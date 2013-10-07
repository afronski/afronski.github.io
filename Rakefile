require 'rubygems'
require 'rake'
require 'rdoc'
require 'date'
require 'yaml'
require 'tmpdir'
require 'jekyll'

desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(
      Jekyll.configuration({
        "source"      => ".",
        "destination" => "_site"
      })
    ).process
  end

desc "Generate and publish blog to gh-pages"
  task :publish => [ :generate ] do
    Dir.mktmpdir do |tmp|
      message = "[Blog] Updated at #{Time.now.utc}."

      system "mv _site/* #{tmp}"
      system "git checkout master"
      system "rm -rf *"
      system "mv #{tmp}/* ."
      system "touch .nojekyll"

      system "git add -u -v"
      system "git add -v ."
      system "git commit -am #{message.shellescape}"
      system "git push origin master --force"
      system "git checkout jekyll"

      system "echo Finished!"
    end
  end

task :default => :publish