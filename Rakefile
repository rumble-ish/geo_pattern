$LOAD_PATH.unshift File.expand_path('../lib', __FILE__)

desc 'Default task running Tests'
task default: :test

desc 'Run test suite'
task test: ['test:rubocop', 'test:rspec']
task 'test:ci' => ['bootstrap:gem_requirements', :test]
namespace :test do
  task :rspec do
    sh 'rspec'
  end

  task :rubocop do
    sh 'rubocop'
  end

  task 'rubocop:autocorrect' do
    sh 'rubocop --auto-correct'
  end

  task 'inch' do
    sh 'inch'
  end
end

namespace :gem do
  require 'bundler/gem_tasks'
end

unless ENV.key?('CI')
  require 'geo_pattern/geo_pattern_task'

  namespace :fixtures do
    string = 'Mastering Markdown'

    GeoPattern::GeoPatternTask.new(
      name: 'generate',
      description: 'Generate patterns to make them available as fixtures',
      data: {
        'fixtures/generated_patterns/boxes.svg'                 => { input: string, patterns: [:chevrons] },
        'fixtures/generated_patterns/cones.svg'       => { input: string, patterns: [:concentric_circles] },
        'fixtures/generated_patterns/circle.svg'                 => { input: string, patterns: [:diamonds] },
        'fixtures/generated_patterns/mummy.svg'                 => { input: string, patterns: [:hexagons] },
        'fixtures/generated_patterns/spiral.svg'           => { input: string, patterns: [:mosaic_squares] },
        'fixtures/generated_patterns/triangle.svg'           => { input: string, patterns: [:nested_squares] },
        'fixtures/generated_patterns/vanishing.svg'                 => { input: string, patterns: [:octagons] }
      }
    )
  end
end

desc 'Bootstrap project'
task bootstrap: %w(bootstrap:bundler)

desc 'Bootstrap project for ci'
task 'bootstrap:ci' do
  Rake::Task['bootstrap'].invoke
end

namespace :bootstrap do
  desc 'Bootstrap bundler'
  task :bundler do |t|
    puts t.comment
    sh 'gem install bundler'
    sh 'bundle install'
  end

  desc 'Require gems'
  task :gem_requirements do |t|
    puts t.comment
    require 'bundler'
    Bundler.require
  end
end
