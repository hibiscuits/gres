# gres
grep, but with substitution

for those who think that:

	gres -rw "pattern" "substitution" .

is more elegant (or at the very least more memorable) than
	
	find . -exec sed -i -e 's/pattern/substitution/g' {} +

or
	
	grep -rPl pattern . | xargs sed -i 's/pattern/substitution/g'

or
	
	perl -pi -e 's/pattern/substitution/g' `grep -rPl pattern .`

It can also be used just like grep:

	tail log.txt | gres "pattern" "\0"

or the shorthand version,

	tail log.txt | gres -g "pattern"

You can even manipulate matches using `{}`-enclosed Python expressions with the -e flag:

	# add 5 to all integers in file
	gres -we "\d+" "{\0+5}" file

- When writing, it automatically creates a backup of the file and then removes it if the write completed without errors.  If an error is encountered, it attempts to restore the backup.  
- Also includes an interactive mode where you can decide, for each matched line, whether to replace it or skip it.
- Requirements: Python 3
- Windows is not supported (yet)

For the future:
- Option to highlight original capture groups when printing `\0`
- Implement more of grep's flags
- Multiline mode

<!-- markdown is dumb sometimes -->

	usage: gres [-h] [-w] [-i] [-r] [-n] [-q] [-g] [-e] [-p] [-H] [-m] [-C num_lines]
	            pattern subst [files [files ...]]

	grep, but with replace

	positional arguments:
	  pattern
	  subst
	  files

	optional arguments:
	  -h, --help    show this help message and exit
	  -w            write substitions to file
	  -i            interactive mode (always write mode, -w is redundant)
	  -r            traverse directory recursively
	  -n            print file names and line numbers (automatic with -r and -i)
	  -q            quiet (only with -w)
	  -g            grep mode; automatically sets the substitution pattern to "\0"
	  -e            exec: perform Python operations matches
	  -p            print each line of input, not just ones with substitutions (overridden by -i and -q)
	  -H            include hidden files and directories (warning: includes .git!)
	  -m            monochrome; no colors or highlighting (automatic if output is redirected)
	  -C num_lines  add [num_lines] of context on either side of each match (overridden by -p and -q)
