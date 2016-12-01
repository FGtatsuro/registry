require 'rake'
require 'rspec/core/rake_task'

namespace :registry do

  def registry_start_cmd(region, bucket)
    env = [
      "REGISTRY_STORAGE_S3_REGION=#{region}",
      "REGISTRY_STORAGE_S3_BUCKET=#{bucket}"
    ]
    command = "#{env.join(' ')} docker-compose up -d"
    return command
  end

  def registry_stop_cmd
    # key id/secret aren't needed to stop service.
    # These dummy values are used only to suppress warnings.
    env = [
      "AWS_ACCESS_KEY_ID=''",
      "AWS_SECRET_ACCESS_KEY=''"
    ]
    command = "#{env.join(' ')} docker-compose down"
    return command
  end

  desc "Generate start command.\n" +
       "This is useful when we want to start servers via Ansible."
  task 'gen-start-cmd', :region, :bucket do |t, args|
    puts registry_start_cmd(args[:region], args[:bucket])
  end

  desc "Start registry."
  task 'start', :region, :bucket do |t, args|
    command =
      "cd #{File.expand_path(__dir__)}/deploy/docker/ && " +
      registry_start_cmd(args[:region], args[:bucket])
    sh command
  end

  desc "Generate stop command.\n" +
       "This is useful when we want to stop servers via Ansible."
  task 'gen-stop-cmd' do
    puts registry_stop_cmd
  end

  desc 'Stop registry.'
  task 'stop' do
    command =
      "cd #{File.expand_path(__dir__)}/deploy/docker/ && " +
      registry_stop_cmd
    sh command
  end
end
