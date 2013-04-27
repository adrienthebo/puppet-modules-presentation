desc 'Build presentation'
task :build do
  sh %{slideshow --h2 --output output --template deck.js presentation.text}
  sh %{cp presentation.text output/presentation.txt}
end

require 'tmpdir'
desc 'Deploy to gh-pages'
task :deploy => [:build] do

  Dir.mktmpdir do |tmp|
    commands = <<-EOS
      cp -r output #{tmp}/
      git checkout gh-pages
      cp -r #{tmp}/output/* .
      cp presentation.html index.html
      git add index.html presentation.html presentation.txt images
      git commit -m 'Update'
      git push
      git checkout master
    EOS

    commands.split("\n").each {|ell| sh ell.strip}
  end
end

task :default => :build
