# frozen_string_literal: true
require 'rake/testtask'

task default: :spec

namespace :credentials do
  require 'yaml'

  desc 'generate access_token to STDOUT'
  task :get_access_token do
    credentials = YAML.load(File.read('config/credentials.yml'))
    require_relative 'lib/acegroups/fb_api'
    ENV['FBAPI_CLIENT_ID'] = credentials[:client_id]
    ENV['FBAPI_CLIENT_SECRET'] = credentials[:client_secret]

    puts "Access Token: #{FaceGroups::FbApi.access_token}"
  end

  desc 'Export sample credentials from file to bash'
  task :export do
    credentials = YAML.load(File.read('config/credentials.yml'))
    puts 'Please run the following in bash:'
    puts "export FBAPI_CLIENT_ID=#{credentials[:client_id]}"
    puts "export FBAPI_CLIENT_SECRET=#{credentials[:client_secret]}"
  end
end

desc 'run tests'
task :spec do
  sh 'ruby spec/facegroups_spec.rb'
end

desc 'delete cassette fixtures'
task :wipe do
  sh 'rm spec/fixtures/cassettes/*.yml' do |ok, _|
    puts(ok ? 'Cassettes deleted' : 'No cassettes found')
  end
end

namespace :quality do
  desc 'run all quality checks'
  task all: [:rubocop, :flog, :flay]

  task :flog do
    sh 'flog lib/'
  end

  task :flay do
    sh 'flay lib/'
  end

  task :rubocop do
    sh 'rubocop'
  end
end
