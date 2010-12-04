require 'rubygems'
require 'jekyll'
require 'ftp_sync'

task :default => 'tags:generate_page'

# Found at: http://gist.github.com/143571
namespace :tags do
  task :generate_page do
    puts 'Generating tags page'
    include Jekyll::Filters
 
    options = Jekyll.configuration({})
    site = Jekyll::Site.new(options)
    site.read_posts('')
 
    html =<<-HTML
---
layout: default
title: Tags
---
<h2>Tags</h2>
    HTML
 
    # Sort by the number of posts in the category.
    tags = site.tags.sort_by { |s| s[1].length }

    tags.reverse.each do |tag, posts|
      html << <<-HTML
      <a name="#{tag}"><h3 id="#{tag}">&rarr; #{tag} (#{posts.length})</h3></a>
      HTML
 
      html << '<ul class="posts">'
      posts.reverse.each do |post|
        post_data = post.to_liquid
        html << <<-HTML    
          <li><span>#{date_to_string post.date}</span>: <a href="#{post.url}">#{post_data['title']}</a></li>
        HTML
      end
      html << '</ul>'
    end
 
    File.open('tags.html', 'w+') do |file|
      file.puts html
    end
 
    puts 'Done.'
  end

  task :generate_cloud do
    puts 'Generating tag cloud'
    include Jekyll::Filters
 
    options = Jekyll.configuration({})
    site = Jekyll::Site.new(options)
    site.read_posts('')
 
    categories = site.tags.sort_by { |s| s[0].downcase }
    html = ""

    categories.each do |tag, posts|
      html << <<-HTML
      <a href="/tags.html##{tag}">#{tag.downcase}<!-- (#{posts.length}) --></a> 
      HTML
    end
 
    File.open('_includes/tagcloud.html', 'w+') do |file|
      file.puts html
    end
 
    puts 'Done.'
 
  end
end

task :upload do
  puts 'Upload _site to live server'
  puts 'Please enter username: '
  user = STDIN.gets.chomp
  puts 'Please enter password: '
  password = STDIN.gets.chomp
  
  ftp = FtpSync.new('www.linnemann.me', user, password)
  ftp.sync('_site', '/')

  puts "done."
end
