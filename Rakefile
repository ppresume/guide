require 'rake/clean'

# `rake clean`
CLEAN.include('\#*\#', '.\#*', '*~', '**/*.aux', '**/*.log', '**/*.out', '**/*.toc')

# `rake clobber`
CLOBBER.include('**/*.pdf', '**/*.tex')

rule '.tex' => '.org' do |t|
  sh "pandoc --toc -N -V classoption:titlepage --latex-engine xelatex -H pandoc/ctex.tex.pandoc.header #{t.source} -o #{t.name}"
end

rule '.pdf' => '.tex' do |t|
  sh "xelatex #{t.source}"
end

source_files = Rake::FileList.new("**/*.org")

task :default => :pdf
desc "Build pdf files from tex file"
task :pdf => source_files.ext('.pdf')
desc "Build pdf files from tex file"
task :tex => source_files.ext('.tex')
