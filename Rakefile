require "pathname"

root_dir = Pathname.new(__FILE__).dirname
integration_test_dir = root_dir + "tests" + "integration"
integration_test_dirs = Pathname.new(integration_test_dir)
                                .children.select(&:directory?)

task default: %w(test)

desc "run all tests"
task :test do
  integration_test_dirs.each do |d|
    rakefile = d + "Rakefile"
    if rakefile.exist? && rakefile.file?
      Dir.chdir(d) do
        puts format("entering to %s", d)
        begin
          puts "running rake"
          sh "rake"
        ensure
          sh "rake clean"
        end
      end
    else
      puts "Rakefile does not exist, skipping"
    end
  end
end

task :clean do
  integration_test_dirs.each do |d|
    rakefile = d + "Rakefile"
    next unless rakefile.exist? && rakefile.file?
    Dir.chdir(d) do
      puts format("entering to %s", d)
      begin
        puts "running rake clean"
        sh "rake clean"
      rescue StandardError => e
        puts "rake clean clean failed:"
        puts e.message
        puts e.backtrace.inspect
      end
    end
  end
end
