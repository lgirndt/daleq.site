require 'rake/clean'

CLEAN.include('_site','assets/css')
CSS_DIR = 'assets/css'

CSS_TARGETS = FileList[CSS_DIR+'/style.css']

directory CSS_DIR

# Compile less from less/* to assets/css
rule '.css' => [ proc { |tn| tn.sub(/\.css$/, '.less').sub(/^assets\/css\//,'less/') }, CSS_DIR ] do |t| 
	sh "lessc #{t.source} #{t.name}"
end	

task :default => [:minify]

task :css => [CSS_TARGETS]

task :minify => :css do
	sh "juicer merge -f #{CSS_TARGETS.join(' ')}"
end


task :serve do
	system('jekyll --server --auto --base-url /')
end	