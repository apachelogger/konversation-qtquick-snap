SOURCES = [
  'deb http://archive.neon.kde.org/dev/unstable xenial main'
].freeze

task :'repo::setup' do
  File.open('/etc/apt/sources.list.d/neon.list', 'w') do |f|
    SOURCES.each { |line| f.puts(line) }
  end
  sh 'apt-key adv --keyserver keyserver.ubuntu.com --recv 55751E5D'
  sh 'apt update'
end

task :snapcraft do
  # KDoctools is rubbish and lets meinproc resolve asset paths via QStandardPaths
  ENV['XDG_DATA_DIRS'] = "#{Dir.pwd}/stage/usr/local/share:#{Dir.pwd}/stage/usr/share:/usr/local/share:/usr/share"
  sh 'apt install -y snapcraft'
  sh 'snapcraft --debug'
end
task :snapcraft => :'repo::setup'

task :publish do
  require 'fileutils'
  sh 'apt update'
  sh 'apt install -y snapcraft'
  cfgdir = Dir.home + '/.config/snapcraft'
  FileUtils.mkpath(cfgdir)
  File.write("#{cfgdir}/snapcraft.cfg", File.read('snapcraft.cfg'))
  sh 'snapcraft push *.snap --release candidate,beta,edge'
end
