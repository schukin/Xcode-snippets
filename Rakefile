CODE_SNIPPET_PATH = File.expand_path('~/Library/Developer/Xcode/UserData/CodeSnippets/')

desc 'Copies all the snippets into Xcode'
task :default do
	`cp *.codesnippet #{CODE_SNIPPET_PATH}.`
	puts "Snippets have been copied to #{CODE_SNIPPET_PATH}"
end

desc 'Show the most recently edited snippet in Xcode'
task :show_recent do
	files_sorted_by_time = Dir.glob("#{CODE_SNIPPET_PATH}/*").sort_by{ |f| File.mtime(f) }.reverse
	most_recent = files_sorted_by_time.first
	puts `cat #{most_recent}`
	puts "==========================="
	puts files_sorted_by_time.first
end