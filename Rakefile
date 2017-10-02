require 'yaml'

desc "Custom: Validate manifests, (erb/epp)templates, and ruby files"
task :check do
    Dir['*.json'].each do |template|
          sh "packer validate #{template}"
            end

    #Dir['puppet/**/*.pp'].each do |manifest|
        sh "puppet parser validate --noop puppet/manifests/site.pp"
    #end

    Dir['puppet/hiera/**/*.yaml'].each do |yaml_file|
     sh "ruby -e \"require 'yaml'; YAML.load_file('#{yaml_file}')\""
    end
end

desc "Custom: Prepare symbolic link to root puppet module folders"
  task :symlink do
  system 'ln -s "/packer_home/iso" "iso"' if not Dir.exist?('iso')
end

desc "Custom: Download third-party modules"
task :r10k do
  puts "\033[1;33mChecking Dependency Modules#\033[0m"
  system 'r10k puppetfile check'
  system 'r10k puppetfile purge'
  system 'r10k puppetfile install -v'
end

task :default => [:r10k, :symlink, :check]
