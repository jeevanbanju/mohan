[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-7f7980b617ed060a017424585567c406b6ee15c891e84e1186181d67ecf80aa0.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=12043413)
# A1: Pokemon Card Parser

Learning Outcomes
-----------------

1.  **Write** a Makefile to compile a simple C program.
2.  **Use** file stream functions to open and read files.
3.  **Create** a program that will parse file content for display.
4.  **Use** memory allocator functions.

Specifications
--------------

For this assignment, you will complete a program that parses a .csv file full of Pokemon card data, as well as a .csv file full of their respective ability data. The reason that the abilities data is split into another file is because many pokemon share the same abilities between each other. It is more efficient to store the abilities data seperately and use pointers to the data when a pokemon needs it as this method does not duplicate data. The program will detect invalid pokemon entries, remove them, sort and then nicely print out the pokemon. 

### Stream I/O

You may use raw file streams for this assignment. Gone are the dark ages of having to deal with a too-tiny buffer!

### Array of Pointers to Structs

The data structure we want to use for this assignment is **a dynamic array of `POKEMON_T*`** or pointers to `POKEMON_T` structs. That means the data type of `pokemon_structs` needs to be `POKEMON_T**`.
You will also implement a similar dynamic array of `ABILITY_T*` this will have pointers to ABILITY_T structs.

### Obtaining a Line from the File

I strongly recommend using the `getline()` function. It basically does everything for you, the trick is that you have to figure out how to call it properly. Don't forget to `free()` the memory allocated by the function before you exit the program or you will have a Valgrind error!

### Parsing the Line

Parsing the abilities line is simple as each line in the csv is very short, with only name and desc per line.  
Parsing the pokemon line is a little more tricky. A pokemon can have 1-3 abilities. If it has multiple, the last ability is a hidden ability. Once you have read the name of the ability you will search for that ability in the `ABILITY_T*` array. The field in the pokemon struct for each ability is a pointer to the ability data in the abilities struct. If a pokemon does not have all 3 abilities, set the missing abilities to be a null pointer. The height and weight fields are also a little weird. They are in decimeters and decagrams. So you should convert them to meters, and kilograms, then turn that string representation of the number into a float. For example, if the height in the .csv is "60" that should be 0.6m. If the weight is "4", that should be 0.4 kg. Some lines of the csv do not have pokemon data, only a name and offset to the ascii art. If a line like this is detected you should immediately return NULL. 

### Restrictions

*   You must compile your program using the flags `std=gnu11`, `-Werror` and `-Wall`.
*   You must implement the stubbed functions: `pretty_print()`, `parse_pokemon()`, `parse_ability()`

### Submission

**Submit only your completed `parser.c` file, `card.h` and associated `Makefile`.** When `make` is executed in the directory, a binary named `parser` should be created. If not, the autograder will fail. You can make a submission as many times as you'd like in order to see the autograder feedback. The maximum points you can receive from the autograder is XXX.

### Functions

Here is a list of functions that are used by the solution program. This might help in completing the assignment.

*   `pretty_print()`
*   `parse_pokemon()`
*   `parse_ability()`
*   `sort_abilities()`
*   `sort_pokemon()`
*   `sort_comp()`
*   `main()`
*   `printf()`
*   `fopen()`
*   `realloc()`
*   `qsort()`
*   `getline()`
*   `free()`
*   `fclose()`
*   `malloc()`
*   `strdup()`
*   `strsep()`
*   `atoi()`
*   `strcasestr()`
*   `bsearch()`
*   `strcat()`
*   `memset()`
*   `atof()`

Example
-------

Here's sample out from a working program. ***Your program must not print out anything other than what's shown or the autograder will fail***. The `$` character represents the terminal prompt, you do not type this character when executing commands.
 
    $ ./parser pokemon.csv 0
        #460 Abomasnow
    [1mGrass[0m / [1mIce[0m
    
    [1mAbilities:[0m
    1. Snow Warning
       The Pokémon summons a hailstorm in battle
    [3m2. Soundproof (hidden)
       Avoids sound-based moves[0m
    
    HP: 90		| 135.50kg
    Atk: 92		| 2.20m
    Def: 75		|
    Sp. Atk: 92	|
    Sp. Def: 85	|
    Speed: 60	| XP: 173
    
    #063 Abra
    [1mPsychic[0m
    
    [1mAbilities:[0m
    1. Synchronize
       Passes on status problems
    2. Inner Focus
       Prevents flinching[0m
    [3m3. Magic Guard (hidden)
       The Pokémon only takes damage from attacks[0m
    
    HP: 25		| 19.50kg
    Atk: 20		| 0.90m
    Def: 15		|
    Sp. Atk: 20	|
    Sp. Def: 55	|
    Speed: 90	| XP: 62
    
    #359 Absol
    [1mDark[0m
    
    [1mAbilities:[0m
    1. Pressure
       Raises foe’s PP usage
    2. Super Luck
       Heightens the critical-hit ratios of moves[0m
    [3m3. Justified (hidden)
       Raises Attack when hit by a Dark-type move[0m
    
    HP: 65		| 47.00kg
    Atk: 130	| 1.20m
    Def: 60		|
    Sp. Atk: 130	|
    Sp. Def: 60	|
    Speed: 75	| XP: 163
    
    #617 Accelgor
    [1mBug[0m
    
    [1mAbilities:[0m
    1. Hydration
       Heals status problems if it is raining
    2. Sticky Hold
       Prevents item theft[0m
    [3m3. Unburden (hidden)
       Raises Speed if a held item is used[0m
    
    HP: 80		| 25.30kg
    Atk: 70		| 0.80m
    Def: 40		|
    Sp. Atk: 70	|
    Sp. Def: 60	|
    Speed: 145	| XP: 173
    
    #142 Aerodactyl
    [1mRock[0m / [1mFlying[0m
    
    [1mAbilities:[0m
    1. Rock Head
       Protects the Pokémon from recoil damage
    2. Pressure
       Raises foe’s PP usage[0m
    [3m3. Unnerve (hidden)
       Makes the foe nervous and unable to eat Berries[0m
    
    HP: 80		| 59.00kg
    Atk: 105	| 1.80m
    Def: 65		|
    Sp. Atk: 105	|
    Sp. Def: 75	|
    Speed: 130	| XP: 180
    
    ... SNIP ...
    
    #263 Zigzagoon
    [1mNormal[0m
    
    [1mAbilities:[0m
    1. Pickup
       May pick up items
    2. Gluttony
       Encourages the early use of a held Berry[0m
    [3m3. Quick Feet (hidden)
       Boosts Speed if there is a status problem[0m
    
    HP: 38		| 17.50kg
    Atk: 30		| 0.40m
    Def: 41		|
    Sp. Atk: 30	|
    Sp. Def: 41	|
    Speed: 60	| XP: 56
    
    #571 Zoroark
    [1mDark[0m
    
    [1mAbilities:[0m
    1. Illusion
       Comes out disguised as the Pokémon in back
    
    HP: 60		| 81.10kg
    Atk: 105	| 1.60m
    Def: 60		|
    Sp. Atk: 105	|
    Sp. Def: 60	|
    Speed: 105	| XP: 179
    
    #570 Zorua
    [1mDark[0m
    
    [1mAbilities:[0m
    1. Illusion
       Comes out disguised as the Pokémon in back
    
    HP: 40		| 12.50kg
    Atk: 65		| 0.70m
    Def: 40		|
    Sp. Atk: 65	|
    Sp. Def: 40	|
    Speed: 65	| XP: 66
    
    #041 Zubat
    [1mPoison[0m / [1mFlying[0m
    
    [1mAbilities:[0m
    1. Inner Focus
       Prevents flinching
    [3m2. Infiltrator (hidden)
       Passes through the foe’s barrier and strikes[0m
    
    HP: 40		| 7.50kg
    Atk: 45		| 0.80m
    Def: 35		|
    Sp. Atk: 45	|
    Sp. Def: 40	|
    Speed: 55	| XP: 49
    
    #634 Zweilous
    [1mDark[0m / [1mDragon[0m
    
    [1mAbilities:[0m
    1. Hustle
       Trades accuracy for power
    
    HP: 72		| 50.00kg
    Atk: 85		| 1.40m
    Def: 70		|
    Sp. Atk: 85	|
    Sp. Def: 70	|
    Speed: 58	| XP: 147
    
    $ ls
