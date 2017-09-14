## Outreachy - Git format-languages Project Proposal

**Author :** Phionah Bugosi <br />
**Email :** bugosip@gmail.com <br />
**IRC Nick :** Phionah <br />

### Background to the project

As defined by [wikipedia](https://en.wikipedia.org/wiki/Git), Git is a version control system for tracking changes in computer files and coordinating work on those files among multiple people. It is primarily used for source code management in software development,[8] but it can be used to keep track of changes in any set of files. As a distributed revision control system it is aimed at speed,data integrity, and support for distributed, non-linear workflows.

Most Git commands take a --format option to allow you to specify a custom output format. An example is these commands.

+ git log that shows the output of commit logs
+ git for-each-ref that shows the output information on each ref and commits.
+ git cat-file --batch-check that gives content or type and size information for repository objects

git log and git for-each-ref take the --format as an option but using different syntax to return the required information in each scenario. git cat-file takes the --batch-check option to return the required information as well.

The goal of the project is to ensure these commands use the same syntax for consistency and in accepted cases same items. To appreciate their polarities, Let me discuss the differences that exist with these commands.

### Existing differences in the commands

#### Arguments that --format takes - git log && git for-each-ref

When the --format option is used with the commands git log and git for-each-ref, it returns diffrent information.

The --format option is used with git log, it takes as arguments one of oneline, short, medium, full, fuller, email, raw, format:<string> and tformat:<string>. For example this:

	git log --format=short

Returns relatively summarised log information.

	Merge: c739cd1 94c9fd2
	Author: Junio C Hamano <gitster@pobox.com>

	    Sync with maint

It takes a --pretty format and returns relevant output.


When using --format option  with git for-each-ref, we are allowed to give a string as argument. For example this:

	git for-each-ref --format='%(subject)'

Returns information regarding reference name and commit messages.

	master Sync with maint
	origin/HEAD Sync with maint
	origin/maint RelNotes: further fixes for 2.14.2 from the master front
	origin/master Sync with maint
	origin/next Sync with master

It intepretes the parameters in the %(). The parameters allowed are the accepted ref and commit information parameters like refname and subject.

**Difference one :** When using --format with git for-each-ref, the syntax does allow using the --pretty format arguments like short, medium etc but the results are different and not as intended.

	git for-each-ref --format=short

Returns:

	short
	short


and using --format with git log given a string value just replaces the log values with the string for example this:

	git log  --format='%(Author:short)'

Returns this times the number of available commits:

	%(Author:short)
	%(Author:short)

**Difference Two :** It is also evident that these two commands return different output named with different parameters in some cases. Needs confirmation on what is called what in each case. The commit message is named subject for git for-each-ref but may not be so for git log.

### Proposed Implementation Road Map

1. The idea is to ensure both commands take as parameters the --pretty format arguments that is to say one of oneline, short, medium, full, fuller, email, raw, format:<string> and tformat:<string> and return output fomatted accordingly with expected output. Therefore the syntaxes:

	git log --format=short
	git for-each-ref --format=short

Should all be accepted and should return required output.


2. Both commands should also be able to take a string as --format argument and interprete the variables as accepted in the string. Therefore as an example this syntax:

	git log  --format='%(subject)'
	git for-each-ref --format='%(subject)'

Should be acceptable for both commands and should return same data akin to this:

	Sync with maint
	Sync with maint
	RelNotes: further fixes for 2.14.2 from the master front
	Sync with maint
	Sync with master

Depending on your project commits, the output may be different to mine but the idea is both commands return the same output.

3. git cat-file when used with the --batch-check option should also take --pretty format arguments and string arguments returning output that is formated in the same way git log && and git for-each-ref does return output.

### Scope

The unification process shall affect three git commands:

+ git log 
+ git for-each-ref
+ git cat-file --batch-check


### Time Line

Tentatively this is the work plan with details on how long and when the project broken down in small tasks shall be executed.

05 - 12 - 2017  to 05 - 01 - 2018

+ Implement unification for the git log command i.e ensure string argument works like in git for-each-ref
+ Review for this milestone
+ Work on reviews
+ Write blog

06 - 01 - 2018  to 06 - 02 - 2018

+ Implement unification for the git for-each-ref command i.e ensure --format takes --pretty format arguments (short, full etc)
+ Review for this milestone
+ Work on reviews
+ Work on reviews
+ Write blog

06 - 12 - 2017  to 05 - 01 - 2018

+ Implement unification for the git cat-file command i.e ensure --batch-check takes --pretty format arguments (short, full etc)
+ Review for this milestone
+ Work on reviews
+ Write wrapp up blog

### Challenges
+ I need to dig around the code base to know how the pieces fit together before ai can know how to actually unify the syntax for these commands

Conclusion

In this document, I give a road map based on what I currently understand about the project in question. I stand to be corrected and welcome suggestions. You can open a pull request to state any information you think I have missed out or need to know.

 











