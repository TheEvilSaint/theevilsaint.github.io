---
layout: post
title: "Ruby Tutorial"
date: "2021-09-17"
description: ""
coverimage: ruby_tutorial.jpg
tags: ruby
published: true
posttype: article
categories: tutorial
---
# Ruby

## Gems

Getting help with gems
```
gem help
```

Searching for a gem
```
gem search [string]
```

Search remotely for a gem related to http
```
gem search -r http
```

To get help searching
```
gem search -h
```

To install a gem
```
gem install [gem name]
gem install openssl-extensions
```

Help is available with install also
```
gem install -h
```

To list locally installed gems
```
gem list
```

## Variable Scopes

| Variable | Scope          |
| -------- | -------------- |
| @var     | instance scope |
| @@var    | class scope    |
| $var     | Global scope   |

## Time and Date

Assign Time right now to a variable
```ruby
time = Time.now
```

## String Methods

Some example string methods
```ruby
st = "a string for me to be testing"
st.empty?
st.clear
st.length
st.size
st.start_with? "a"
st.eng_with? "ing"
st.insert(10,"cat") # insert the word cat at 10th char
st.upcase
st.downcase
st.capitalize
st.reverse
st.chop      # removes whatever the trailing char is
```

Using pry we could type "st." and then use tab completion to show the possibilities

Referencing parts of a string and assigning values to parts of a string

Assuming `st = "this is my string"`

replaces the word string with code
```ruby
st["string"] = "code"
```

replaces the word "this" with "that"
```ruby
st[1..4] = "that"
```

### Sub and Gsub

Sub replaces the first occurrence of a string while Gsub replaces all occurrences. Assuming `st = "No one is like PHP. PHP is awesome"`
```ruby
>> st.sub("PHP","Ruby")
=> "No one is like Ruby. PHP is awesome"

>> st.gsub("PHP","Ruby")
=> "No one is like Ruby. Ruby is awesome"
```

> Note, sub and gsub just return a copy of the string with the proper substitution. If you want to modify the original string you have to use sub! or gsub!

Make a permenant change to the variable
```ruby
st.sub!("Search","Replace")
st.gsub!("Search","Replace")
```

### Freeze and Frozen

It is  possible to make a string immutable (prevent it from being changed). We can do this with the freeze method. The freeze method works on an object, not the variable that refers to a frozen object.

Freeze (make an object immutable)
```ruby
st.freeze
```

Test if object has been frozen with the freeze method
```ruby
st.frozen?
```

For example
```ruby
st = "i will make this immutable"
st.freeze
st.concat("trying to append") # this won't work, string itself cannot be altered
```

> We can change the existing variable but we can assign a new value over the top like this `st = "new value completely"`


### Bypassing String Escapes

Write strings without the quotation delimiters
```ruby
print %[delimiter is using the brackets]
```

A Ruby Heredoc (in this example EVILSAINT is the delimiter)
```ruby
St = <<EVILSAINT
This is whatever I want it to be with whatever formatting
I would like " " ' / \
Over multiple lines too….
Yeah for             spaces and           heredocs
EVILSAINT
```

### Concatenating strings (various methods):

+ notation
```ruby
St = "Ruby" + " is an " + "awesome language"
```

Juxtaposition
```ruby
"Hello " "World" "!!!!!!!"
```

<< notation
```ruby
"You " << "are " << "Welcome!"
```

OO Style
```ruby
"Ruby ".concat("is ").concat("awesome!")
```

## Arrays

The following methods can all be used to access different elements of the `days` array
```ruby
days = ["mon","tues","wed","thu","fri","sat","sun"]
days[3]
days[-1]
days[-2]
days.at(0)
days.first
days.last
days[3..5]
```

We can inset into arrays (here we inset the number 10 into the first element)
```ruby
a = [ 7, 3, 8]
a.insert(1, 10)
```

We can also delete. Here is two ways of achieving the same thing. We can delete the value 10 or we can delete the element at index 1
```ruby
a.delete(10)
a.delete_at(1)
```

Useful array methods
```ruby
a = [1, 10, 4, 32, 5, 88, 11, 32]
a.sort
a.reverse
a.uniq
a.max
a.min
a.join(" ")    # takes an argument to place between items
a.join         # no parameter means array will be joined without
"1:2:3:4:5:6".split ":"  # makes an array from string
```

## File System

List Files
```ruby
Dir["/Home/*.txt"]
```

Copy Files
```ruby
FileUtils.cp('comics.txt', '/Home/comics.txt')
```

Write to Files
```ruby
File.open("/Home/comics.txt", "a") do |f|
    f.puts "put this in file"
end
```

Print File to Screen
```ruby
print File.read("/Home/comics.txt")
```

Find out when file was last modified
```ruby
File.mtime("/Home/comics.txt")
File.mtime("/Home/comics.txt").hour
```

## Define a function

```ruby
def load_comics( path )
  comics = {}
  File.foreach(path) do |line|
    name, url = line.split(': ')
    comics[name] = url.strip
  end
  comics
end
```

## Inheritance

```ruby
class ApplicationError
  def display_error
    puts "Error! Error!"
  end
end

class SuperBadError < ApplicationError
end

err = SuperBadError.new
err.display_error
```

## Object Orientated
```ruby
Class conventions for setters/getters

attr_reader
attr_writer
attr_accessor

Modules

module ModuleName
  # Bits 'n pieces
end
```

## Ruby One Liners

Ruby command line switches

| Option   | Description                                                                                    |
| -------- | ---------------------------------------------------------------------------------------------- |
| -I[.ext] | In place edit. If given an extension it backs up first                                         |
| -e       | Allows you to run a command on the command line as opposed from a script or interactive prompt |

Read a file from command line
```ruby
$ ruby -pe 0 'file'
```

Count the number of lines in a file
```ruby
$ ruby -ne 'END {print "Lines: ",$.,"\n"}' file
```

Make a backup copy of a file and replace all the 'foo' strings with 'FOO'
```ruby
$ ruby -i.bak -pe 'gsub "foo", "FOO"' *.txt
```

Interactive Ruby
```ruby
apt-get install pry
pry --simple-prompt
```

Make a list of IP addresses
```ruby
ARGV[1].upto(ARGV[2]) {|i| print ARGV[0],".",I,"\n"}
2.upto(254) {|i| print "192.168.1",".",I,"\n"}
```

Pinging hosts
```ruby
>> require 'net/ping'
=> true
>> req = Net::Ping::ICMP.new("8.8.8.8")
=> #<Net::Ping::ICMP:0x000055f5127a98f8
```

## Nokogiri

```ruby
require 'open-uri'
require 'nokogiri'
require 'resolv-replace'
eztv = Nokogiri::HTML(open("https://eztv.bypassed.eu/shows/887/the-blacklist/"))

>> eztv.title
#=> "EZTV Proxy Mirror - The Blacklist Torrent Download - EZTV"

eztv.xpath("//a").css(".download_1").each do |link|
	puts "#{link['title']}: #{link['href']}"
end

File.open("blacklist.out","w") do |line|
	eztv.xpath("//a").css(".download_1").each do |link|
		line.puts "#{link['title']} | #{link['href']}"
	end
end

imdb = Nokogiri::HTML(open("http://www.imdb.com/title/tt1405406/episodes?season=1"))
imdb.css("div.eplist div.info div.image")[0]

Debug to look where you are in the documentation
>> links.class.name
#=> "Nokogiri::XML::NodeSet"
```