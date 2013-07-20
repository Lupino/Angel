#!/usr/bin/env rake

def number_of_cores
  `nproc`.chomp
end

def build_flags
  "-j#{number_of_cores}"
end

desc "run ghci scoped to the sandboxed cabal project"
task :ghci => FileList.new(".cabal-sandbox/*packages.conf.d", "cabal-dev/*packages.conf.d") do |x|
  cmd = "ghci -isrc"
  if package_db = x.prerequisites.first
    cmd += " -package-db #{package_db}"
  end

  sh cmd
end

desc "sandbox with cabal (requires cabal >= 0.17)"
task :sandbox do
  sh "cabal sandbox init"
end

desc "install only essential dependencies. build does this for you"
task :install_dependencies do
  sh "cabal install #{build_flags} --only-dependencies"
end

desc "install only essential dependencies. test does this for you"
task :install_dependencies_for_test do
  sh "cabal install #{build_flags} --only-dependencies --enable-tests"
end

desc "just build the project for production"
task :build => [:configure, :install_dependencies] do
  sh "cabal build #{build_flags}"
end

desc "run tests once. consider using guard afterwards for faster feedback"
task :test => :build_for_test do
  sh "cabal test"
end

desc "clean all artifacts generated by cabal"
task :clean do
  sh "cabal clean"
end

task :configure do
  sh "cabal configure"
end

task :build_for_test => [:configure, :install_dependencies_for_test] do
  sh "cabal build #{build_flags}"
end

task :default => :build