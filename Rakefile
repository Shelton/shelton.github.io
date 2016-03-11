
task :setup do
  sh "sudo gem install bundler"
  sh "bundle install --path .bundle"
end

task :preview do
  sh "bundle exec jekyll serve"
end

task :deploy do
  sh "git commit -a && git push"
end
