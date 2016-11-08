require 'rake/clean'
require 'yaml'

# `rake clean`
CLEAN.include('\#*\#', '.\#*', '*~', '**/*.aux',
              '**/*.log', '**/*.out', '**/*.toc')

# `rake clobber`
CLOBBER.include('**/*.pdf', '**/*.tex').exclude('pandoc/**/*')

rule '.tex' => '.org' do |t|
  metadata = YAML::load(File.open(t.name.sub(/\.tex$/, '.yaml')))
  pandoc_item_args = metadata['pandoc']
  pandoc_default_args = '--latex-engine xelatex '
  pandoc_args = pandoc_item_args + ' ' + pandoc_default_args
  sh "pandoc #{pandoc_args} #{t.source} -o #{t.name}"
end

rule '.pdf' => '.tex' do |t|
  sh "xelatex #{t.source}"
end

rule '.jpg' => '.pdf' do |t|
  sh 'mkdir -p images/jpg/'
  sh 'convert -alpha remove -background white -density 300 -quality 300 '\
     "#{t.source} images/jpg/#{t.name}"
end

source_files = Rake::FileList.new('**/*.org')

task default: :pdf
desc 'Build jpg files from pdf file'
task jpg: source_files.ext('.jpg')
desc 'Build pdf files from tex file'
task pdf: source_files.ext('.pdf')
desc 'Build tex files from org file'
task tex: source_files.ext('.tex')
