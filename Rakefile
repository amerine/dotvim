require 'pathname'

task :default do
  puts "update the bundles:"
  puts # blank line
  puts "  $ rake update:bundles"
end

namespace :update do

  BUNDLES = {
    # plugins
    :bufexplorer         => "git://github.com/vim-scripts/bufexplorer.zip.git",
    :snipmate            => "git://github.com/msanders/snipmate.vim.git",
    :specky              => "git://github.com/vim-scripts/Specky.git",
    :tabular             => "git://github.com/godlygeek/tabular",
    :taglist             => "git://github.com/vim-scripts/taglist.vim.git",
    :textobj_user        => "git://github.com/kana/vim-textobj-user.git",
    :textobj_rubyblock   => "git://github.com/nelstrom/vim-textobj-rubyblock.git",
  }

  desc "update any bundles defined in the Rakefile"
  task :bundles do

    bundle_home = Pathname.new( ENV['HOME'] ) + '.vim' + 'bundle'
    mkdir_p bundle_home

    BUNDLES.sort_by {|k,v| k.to_s }.each do |bundle, location|
      target_path = bundle_home + bundle.to_s

      puts "*" * 72
      puts "*** Instaling #{bundle} to #{target_path} from #{location}"
      puts # blank line

      rm_rf target_path

      case location.match( /^(.*?):/ )[1]
      when 'git'
        sh "git clone #{location} #{target_path} > /dev/null"
        rm_rf target_path + '.git'
      when 'http'
        mkdir_p target_path
        sh "cd #{target_path} && curl -s '#{location}' | tar zx -"
      end

      docdir = target_path + 'doc'
      if docdir.exist?
        puts "doc dir exists; tagging"
        sh "vim -u NONE -esX -c 'helptags #{docdir}' -c quit"
      end

      puts # blank line
      if bundle == :snipmate
        puts "Removing the snippets included in snipmate"
        rm_rf Pathname.new( ENV['HOME'] ) + '.vim' + 'bundle' + 'snipmate' + 'snippets'
      end

      puts # blank line
    end
  end # task :bundles

end # namespace :update

