---
layout:     post
title:      用Flying Saucer+Jsoup生成个人博客文集
category: blog
description: 在这里写了3年的博客，积累了102篇博文。想想大学毕业后可以做一个小结，顺便作一些迁移博客的前期准备。
---

在这里写了3年的博客，积累了102篇博文。想想大学毕业后可以做一个小结，一方面把文章作个备份，另一方面想想工作以后可以作些什么改变，比如我正打算把博客迁移到Github或者阿里云的服务器上，加快访问速度和减少维护成本。

原先打算用Latex生成漂亮的文章，但是没有发现合适的API可以用，如果要较完整地保留原博文的格式、图片等信息，自己写一个从HTML到Latex的转码甚是耗时。于是辗转发现一个叫[Flying Saucer](http://code.google.com/p/flying-saucer/)的Java库，可以很好地将带CSS的网页“打印”成PDF，于是花了两天的时间把博客的文章拉取下来制成了一个文集，预览地址：[Book](http://www.wytk2008.net/wordpress/wp-content/uploads/2014/06/Book.pdf)

![Cover](http://www.wytk2008.net/wordpress/wp-content/uploads/2014/06/IMG_0996-1024x768.jpg)

整个过程包括文章爬取，内容提取和生成PDF，均通过Java完成。个人java还比较渣，写得不太好。

	package crawler_wordpress;


	import org.jsoup.Jsoup;
	import org.jsoup.nodes.Document;
	import org.jsoup.nodes.Element;
	import org.jsoup.select.Elements;

	import java.io.*;
	import java.net.SocketTimeoutException;
	import java.sql.SQLException;

	public class FirstCrawler {
	    public static void main(String[] args) throws SQLException, IOException
	    {
	        getPage("http://www.wytk2008.net/life/1304/");
	    }

	    public static void getPage(String URL) throws SQLException,IOException,SocketTimeoutException
	    {
	        boolean succ = false;
	        while(!succ)
	        {
	            try{
	                processPage(URL);
	                succ = true;
	            }
	            catch(Exception e){
	                System.out.println("Main() - Time out Exception.");
	            }
	        }
	    }

	    public static void processPage(String URL) throws SQLException,IOException,SocketTimeoutException
	    {
	        //获取网页
	        Document doc = Jsoup.connect(URL).get();

	        //提取标签，获取主内容块
	        Element content = doc.getElementById("main");
	        Elements links = content.getElementsByTag("a");

	        //提取文章正文
	        Elements articles = content.getElementsByTag("article");
	        Element article = null;
	        for (Element a : articles)
	        {
	            article = a;
	            break;
	        }
	        //提取评论内容
	        Elements comments = content.getElementsByClass("comment-list");
	        Element comment_list = null;
	        for (Element comment : comments)
	        {
	            comment_list = comment;
	        }

	        //处理多余的元素
	        try{
	            //删除修改时间
	            article.select("time.updated").remove();
	            //删除目录块
	            article.select("footer.entry-meta").remove();
	            //删除评论中的头像
	            comment_list.select("img[src]").remove();
	            //删除回复按钮
	            comment_list.select("div.reply").remove();
	        }
	        catch(Exception e)
	        {
	            System.out.println("Nothing to remove.");
	        }

	        String post_id = article.attr("id");
	        String post_name = article.select("h1.entry-title").text();
	        System.out.print("Crawling "+post_id+" ");
	        System.out.println(post_name);
	        //处理不可用于文件名的非法字符
	        if (post_name.contains(":"))
	        {
	            post_name = post_name.split(":")[0];
	        }

	        //HTML头部
	        String header = "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n" +
	                "<!DOCTYPE html PUBLIC \"-//W3C//DTD XHTML 1.0 Strict//EN\"  \"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd\">\n" +
	                "<html xmlns=\"http://www.w3.org/1999/xhtml\">\n" +
	                "<head>\n" +
	                "    <title>"+post_name+"</title>\n" +
	                "    <style type=\"text/css\"> body {font-family:Georgia,SimSun;} p {line-height:25px; text-indent:2em;} "+
	                "img {text-align:center;max-width:640px; height:auto;width:expression(this.width > 640 ? \"640px\" : this.width);}  " +
	                "a { text-decoration:none; } h1 {font-family:SimHei;} ul li{ margin:0; padding-top:5px;} </style>\n" +
	                "</head>";

	        String outputs;
	        if (comment_list != null)
	        {
	            outputs = header +
	                "<body>\n" +
	                article +
	                comment_list +
	                "</body></html>";
	        }
	        else
	        {
	            outputs = header +
	                    "<body>\n" +
	                    article +
	                    "</body></html>";
	        }

	        String fileName = "src/pages/"+post_id+"-"+post_name+".html";
	        FileOutputStream f = new FileOutputStream(fileName);
	        OutputStreamWriter out = new OutputStreamWriter(f,"UTF-8");
	        out.write(outputs);
	        out.flush();
	        out.close();

	        //获取前一篇文章的URL
	        for (Element link : links)
	        {
	            String linkHref = link.attr("href");
	            String linkrel = link.attr("rel");
	            if (linkrel.equals("prev"))
	            {
	                System.out.println("Next Article URL: "+linkHref);
	                getPage(linkHref);
	            }
	        }
	    }
	}
