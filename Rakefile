require 'rake/clean'

CLEAN.include('_site','assets/css')

directory 'assets/css'

# Compile less from less/* to assets/css
rule '.css' => [ proc { |tn| tn.sub(/\.css$/, '.less').sub(/^assets\/css\//,'less/') } ] do |t| 
	sh "lessc #{t.source} #{t.name}"  
end	

task :serve do
	system('jekyll --server --auto --base-url /')
end	