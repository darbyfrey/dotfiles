require 'rake'
require 'erb'

desc "install the dot files into user's home directory"
task :install do
  generate_gitconfig
  
  replace_all = false
  Dir['*'].each do |file|
    next if %w[Rakefile README LICENSE].include? file
    
    if File.exist?(File.join(ENV['HOME'], ".#{file}"))
      if replace_all
        replace_file(file)
      else
        print "overwrite ~/.#{file}? [ynaq] "
        case $stdin.gets.chomp
        when 'a'
          replace_all = true
          replace_file(file)
        when 'y'
          replace_file(file)
        when 'q'
          exit
        else
          puts "skipping ~/.#{file}"
        end
      end
    else
      link_file(file)
    end
  end
end

def generate_gitconfig
  home_dir = ENV['HOME']
  
  print "What is your name? "
  name = $stdin.gets.chomp
  
  print "What is your email? "
  email = $stdin.gets.chomp
  
  File.open(File.join(File.expand_path(File.dirname(__FILE__)), 'gitconfig'), 'w') do |gitconfig|
    gitconfig.write(ERB.new(File.read('templates/gitconfig.erb')).result(binding))
  end
end

def replace_file(file)
  system %Q{rm "$HOME/.#{file}"}
  link_file(file)
end

def link_file(file)
  puts "linking ~/.#{file}"
  system %Q{ln -s "$PWD/#{file}" "$HOME/.#{file}"}
end
