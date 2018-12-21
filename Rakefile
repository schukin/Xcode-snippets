require 'highline/import'
require 'plist'

CODE_SNIPPET_PATH = File.expand_path('~/Library/Developer/Xcode/UserData/CodeSnippets/')
DST_SNIPPET_PATH = File.expand_path('./snippets/')

desc 'Copies all the snippets into Xcode'
task :default do
	copy_local_snippets_to_xcode!
	puts "Snippets have been copied to #{CODE_SNIPPET_PATH}"
end

desc 'Show the most recently edited snippet in Xcode'
task :show_recent do
	most_recent = get_most_recent_snippet_path
	puts `cat #{most_recent}`
	puts "==========================="
	puts most_recent
end

desc 'Move the most recently edited snippet from Xcode to the repo, then copy back to Xcode'
task :sync_recent do
	most_recent = get_most_recent_snippet_path
	puts `cat #{most_recent}`
	puts "==========================="
	puts most_recent

	plist = Plist.parse_xml(most_recent)
	suggested_name = plist['IDECodeSnippetCompletionPrefix']
	base_name = ask("Enter a name for this snippet, without the extension: ") { |q| q.default = suggested_name }
	filename = "#{base_name}.codesnippet"

	exit(1) unless HighLine.agree("About to move this snippet from XCode to this repo, rename it #{filename}, and copy back to Xcode. Please confirm. y/n")

	cmd = "cp #{most_recent} ./snippets/#{filename}"
	puts cmd
	puts `#{cmd}`
	puts "✅  Snippet #{filename} has been copied to local repo"

	copy_local_snippets_to_xcode!
	puts "✅  Snippets have been copied to #{CODE_SNIPPET_PATH}"
end

def copy_local_snippets_to_xcode!
	cmd = "cp #{DST_SNIPPET_PATH}/*.codesnippet #{CODE_SNIPPET_PATH}/."
	puts cmd
	result = `#{cmd}`
end

def get_most_recent_snippet_path
	files_sorted_by_time = Dir.glob("#{CODE_SNIPPET_PATH}/*").sort_by{ |f| File.mtime(f) }.reverse
	most_recent = files_sorted_by_time.first
	return most_recent
end
