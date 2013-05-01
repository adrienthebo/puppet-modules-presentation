def __DIR__
  File.expand_path('..', __FILE__)
end

desc 'Build presentation'
task :build do
  sh %{slideshow --h2 --output output --template reveal.js --config #{__DIR__} presentation.text}
  sh %{cp presentation.text output/presentation.txt}
  sh %{cp output/presentation.html output/index.html}
end

require 'tmpdir'
desc 'Deploy to gh-pages'
task :deploy => [:build] do

  Dir.mktmpdir do |tmp|
    commands = <<-EOS
      git clone --reference #{__DIR__} git@github.com:adrienthebo/puppet-modules-presentation #{tmp}
      ls #{tmp}
      git checkout -b gh-pages origin/gh-pages
      cp -r #{__DIR__}/output/* #{tmp}/
      cp presentation.html index.html
      git add .
      git ci -a -m 'gh-pages update via rake task'
      git push
    EOS

    Dir.chdir(tmp) do
      commands.split("\n").each {|ell| sh ell.strip}
    end
  end
end

task :default => :build
