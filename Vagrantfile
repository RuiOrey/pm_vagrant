# -*- mode: ruby -*-

dir = File.dirname(File.expand_path(__FILE__))

require 'yaml'
require "#{dir}/puphpet/ruby/deep_merge.rb"
require "#{dir}/puphpet/ruby/puppet.rb"

# *** CUSTOM PROJECT CONFIGURATIONS ***
configCustom = {}
# ADD YOUR PROJECTS HERE
configCustom.deep_merge!(YAML.load_file("#{dir}/projects/pm_vewulff/config.yaml")) # add such a row for each project
File.open("#{dir}/puphpet/config-custom.yaml", 'w') {|f| f.write configCustom.to_yaml }


configValues = YAML.load_file("#{dir}/puphpet/config.yaml")

provider = ENV['VAGRANT_DEFAULT_PROVIDER']
if File.file?("#{dir}/puphpet/config-#{provider}.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-#{provider}.yaml")
  configValues.deep_merge!(custom)
end

if File.file?("#{dir}/puphpet/config-custom.yaml")
  custom = YAML.load_file("#{dir}/puphpet/config-custom.yaml")
  configValues.deep_merge!(custom)
end

data = configValues['vagrantfile']

Vagrant.require_version '>= 1.8.1'

eval File.read("#{dir}/puphpet/vagrant/Vagrantfile-#{data['target']}")
