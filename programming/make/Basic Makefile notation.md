	# target/dependancy/command
The most basic Makefile notation looks like this
```makefile
target : dependancy
	command
```

Where `target` is the thing we want to build (which can be called via `make target`), `dependency` is the list of dependencies and the `command` is what we should call after all the dependencies are built. There could be multiple dependencies, but only one target. Dependecies could nest but not loop. So the makefile could look like this

```makefile
dependency1 :
	command1

dependency2 : dependency1
	command2

dependency3 : dependency1
	command3

target_all : dependency3 dependency2
	command_all
```
# Variables
It’s not always right to write every dependency and name as is. In the example above `dependency1` is used 3 times, but what if we want to change it’s name? We would need to change every occurrence, but a simpler way to go would be defining a variable for it
```makefile
COMMON_DEPENDENCY=common_dependency

$(COMMON_DEPENDENCY):
	command1

dependency2 : $(COMMON_DEPENDENCY)
	command2

dependency3 : $(COMMON_DEPENDENCY)
	command3

target_all : dependency3 dependency2
	command_all
```
# Wildcards in names
Most of the times we don’t want to write simple commands for every object there is, so we might want to use wildcards. To build object files with the same name for all `.cpp` files in a directory one might do this
```makefile
%.o : %.cpp
	g++ $^ -o $@
```
Where
- `%` is the wildcard for filename
- `$@` is the variable for the target
- `$^` is the variable for all the dependecies
	- You might use `$<` if you only need the first dependency