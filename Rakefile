DATA_URL = 'http://centrodedescargas.cnig.es/CentroDescargas/equipamiento/lineas_limite.zip'
DATA_ZIP = 'data.zip'
DATA_FOLDER = 'data'

GEOJSON_DIR = 'es'

desc "Download data from CNIG.es"
task :download do
	sh "curl %{url} -o %{zip}" % {:url => DATA_URL, :zip => DATA_ZIP}
end

desc "Unzip downloaded data"
task :unzip => [:download] do
	sh "unzip %{zip} -d %{unzipped}" % {:zip => DATA_ZIP, :unzipped => DATA_FOLDER}
end

desc "Convert shapefiles to geoJSON"
task :convert do
	input_files = Dir.glob "#{DATA_FOLDER}/**/*.shp"
	mkdir_p GEOJSON_DIR
	input_files.each do |file|
		filename = File.join(GEOJSON_DIR, File.basename(file, '.shp'))
		sh "ogr2ogr -f GeoJSON -t_srs crs:84 #{filename}.geojson #{file}"
	end 
end

task :default => [:download, :unzip, :convert]