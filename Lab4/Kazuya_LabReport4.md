# Lab Report 4  

## 1. Logging into ieng6
**Keys Pressed:** `<up> <Enter>`

**Explanation:** I had just ran the ssh command so I used the arrow keys to scroll up to the most recently ran command and executed it to ssh into ieng6.  

## 2. Clone your fork of the repository from your Github account (using the SSH URL)  
**Keys Pressed:** `<g> <i> <t> <SPACE> <c> <l> <o> <n> <e> <SPACE> <RIGHT CLICK> <ENTER>`  
**Explanation:** I used git to clone of the fork created using the SSH key, using right click to paste the copied directory. 

## 3. Run the tests, demonstrating that they fail  
**Keys Pressed (1):** `<c> <d> <SPACE> <l> <a> <b> <7> <ENTER>`  
**Explanation  (1):** To access the copied repository, I use cd to shift the working directory to ./lab7  
**Keys Pressed (2):** `<b> <a> <s> <h> <SPACE> <t> <e> <s> <t> <.> <s> <h> <ENTER>`  
**Explanation  (2):** Since there is a prewritten script for running the tests, I use bash to execute the test.sh script.  

## 4. Edit the code file to fix the failing test  
**Keys Pressed (1):** `<v> <i> <m> <SPACE> <L> <TAB> <.> <TAB> <ENTER>`  
**Explanation  (1):** To edit the file, I enter the vim command. Entering the first letter of "ListExamples.java", I use tab to auto fill the file name and the file type.  
**Keys Pressed (2):** `<:> <4> <4> <ENTER> <e> <x> <i> <2> <esc> <:> <w> <q> <!> <ENTER>`  
**Explanation  (2):** By entering `:44`, I jump to Line 44 of the file. `e` allows me to use motion to reach the end of the current word. `x` allows me to delete the current letter that my cursor is on, making "index1" -> "index". Then I enter insertion mode by entering `i`, using it to change the "index" to "index2" by clicking `2`. Clicking the `esc` key allows me to return to normal mode, at which I enter `:wq!` to save and exit vim.  

## 5. Run the tests, demonstrating that they now succeed  
**Keys Pressed:** `<up> <up> <ENTER>`   
**Explanation:** I go up my history 2 command by clicking the up arroow twice, allowing to run `bash test.sh` again to run the tests.  

## 6. Commit and push the resulting change to your Github account (you can pick any commit message!)
**Keys Pressed (1):** `<g> <i> <t> <SPACE> <a> <d> <d> <SPACE> <-> <A>`
**Explanation  (1):** Add the modified ListExamples.java file to the staging area by using the git add command, adding -A to specifcy all modified files.  
**Keys Pressed (2):** `<g> <i> <t> <SPACE> <c> <o> <m> <m> <i> <t> <SPACE> <-> <m> <SPACE> <"> <M> <o> <d> <i> <f> <i> <e> <d> <SPACE> <L> <i> <s> <t> <E> <x> <a> <m> <p> <l> <e> <s> <.> <j> <a> <v> <a> <"> <ENTER> `   
**Explanation  (2):** Using the `commit` command, I take the staged edits and add it to the local main branch as specified with `-m`. I also added a message to detail what happened in the commit.   
**Keys Pressed (3):** `<g> <i> <t> <SPACE> <p> <u> <s> <h> <ENTER>`  
**Explanation  (3):** Finally, I push the commit to the remote fork of the repository. 
