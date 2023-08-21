# CICD-notes
------------------------------------------------
Jenkins Shared Library

Jenkins Shared Lib: Enable the pipeline code reusability where the same code is used for multiple pipelines. Also makes it easy to make the changes at a central place and propagate to all the pipelines.

Create groovy functions and call as and when needed from shared lib.

A specific folder structure with 3 top level folders vars, resources, src.
Vars hosts the files exposed as a variable (functions). .groovy extension is used for logic and .txt file with same name as documentation.
You can have global level, folder level shared libraries. Folder based lib are considered not trusted and run in groovy sandbox.

To call the shared library use the annotation @Library at the top of your pipeline. E.g. @Library(‘my-shared-lib’)  _  ( ‘ _’ ) mean give me everything under the shared lib.

To add global shared lib. Goto manage Jenkins > configure system > global pipeline libraries > add 
Assign the name which you will reference in pipeline file
Default version can be a branch name, tag, commit hash. Select modern scm>git copy the link of the remote repo. 

Should keep shared library small in size as everytime a pipeline is run the entire shared library is cloned into your workspace which consumes disk space
When calling functions we can also pass the variable to the function, if the function is defined with input variables.

Examples
def call () {
sh “echo hello world”
}
To call above in pipeline script put: helloWorld()

def call (String name, String dayOfWeek) {
sh “echo Hello ${name}. today is ${dayOfWeek}”
}

To call above in pipeline script put: helloWorld(“Darin”, “Monday”)

to use map instead of positional parameters
Def call (Map config = [:]) {
sh “echo Hello ${config.name}. today is ${config.dayOfWeek}”
}
To call above in pipeline script put: helloWorld(name:“Darin”,  dayOfWeek:“Monday”)

