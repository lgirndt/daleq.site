require 'rake/clean'

CLEAN.include('_site')

rule '.css' => [ proc { |tn| tn.sub(/\.css$/, '.less')} ] do |t|
	sh "echo source #{t.source}, name #{t.name}"  
end	

task :lessc do 
	system('lessc bootstrap/less/bootstrap.less assets/css/bootstrap.css')	
end

task :serve do
	system('jekyll --server --auto --base-url /')
end	