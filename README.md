# Guide Line Code Complete


## Part I: Laying Foundation

- Constructing process: Design, Coding, Debugging, Integration, Testing
- Software construction = Building software. Building dog house != building skyscraper
- Measure twice, cut once. Try to detect defect early (architecture phase or design phase) instead on coding phase.
- Having a good problem definition. Having a good requirements.
- Architecting is about defining resources and constraints.  Eg: Program Organization, Major Class Design, Data Design (eg: this column should be integer), Business Rule, UI Design, Resource Management, Security, Performance, Scalability, Interoperability, Internationalization/Localization, I/O, Error Processing


## Part II: Creating High Quality Code

### A.  Design In Construction
- Level of design: Software system, Division to subsystem/package, Division into classes within package, Division into data and routines with classes, Internal routine design
- Information Hiding: Restrict public method, hide status in enumerate, and hide accessing variable to routine
- Draw UML or Explain in English before writing code
- Design is iterative process. Try to get to the previous point to check is design is valid or not. Try to divide and conquer process.

### B. Working Classes
- Use it to create ADT (Abstract Data Type), when attributes are complex.  Eg: current_font, stack
- Try favor to read-time instead write time. Create abstraction and encapsulation that easy to read. Be consistent about the layer of interface.
- Containtment = “Has a” relationship. Usually use interface in C# or Java.  Eg: telemarketer and mosquito “has a” annoying people method. (Real life is IDisposable interface or Mixins in Python)
- Inheritance = “Is a” relationship. Usually use inheritance from class/base class.  Eg: telemarketer and peddler “is a” person and have method speak. (Real life is Views in Django)
- Class name should be noun. Avoid verbs. If it related to verbs, consider to create function only
- Liskov: Derived class should be agree with all method from parent class (is a). Interface Segregation: Dont put unused Interface to your class (has a).

### C. High Quality Routine
- Routine should be do one thing. If it possible, dont use any condition/flag at routine
- Dont use vague verbs, like perform_service. What kind of service? Maybe reconsider like calculate_salary.
- Routine params should be maxed 5, Not too long, and have order like in, out, and format. Because out is usually at return value, so it should be params_input, and params_format
- Never use params as working variable

### D. Defensive Programming
- Assert for expected internal value, try-catch for user input
- Handle between correctness and robustness. Correctness, value must be correct, otherwise crash. Robustness normalize output when value is wrong.  Eg: int to 0, string to “”.
- We can add baricade to clean up data before processing it.
- Even “Exception” should be in correct abstraction. Eg: UserNotFoundException
- Offensive programming: On development, make error is painful enough so we should fix it.

### E. Pseudocode Programming Process
- Use English to describe the problem, don’t use any syntax or computer words. Describe what todo instead what to implement
- After creating routine, mentally checking routing. Do not depends on compiler. Because this leading to mental hack


## Part III: Variables

- Initialize variables as close as possible to where they’re first used
- Try to make variable short span (the usage between variable) and short live time (when the last variable used).  Eg: variable employee are used in  line 1, 3, 5, 7. So span average is 2, and live time is 7.
- Group related statement to one block. It is shortening span and live time, also easier to refactor (extract method).  Eg: declare var1, use var1, modify var1, declare var2, use var2, modify var2 -> is prefered  Instead of: declare var1, declare var2, use var1, use var2,  modify var1, modify var2.
- Use variable for one purpose. Never use variable name like temp and reassign it with another purpose
- Avoid variable with hidden meaning.  Eg: page_count for number of page printed, but also has flag -1 when indicating errors.  This make page_count has 2 type of data, integer and boolean.
- Considering length of name, not too long, and obvious enough to understand
- Express a good name in “what” terms, not “how”.  Eg: employeeData is better than inputRecord, printerReady is better than bitFlag
- Be consistent between aggregation name and object name.  Eg: total_revenue should not be used with revenue_total. Use one only but not both (writter suggest revenue_total, `<object>_<aggregate>`). I think the reason is to grouping by object first, then know the function is
- If use abbrevation, use pronouncable abbrevation. Avoid ambiguous words.


## Part IV: Statements

### A. Using Conditionals
- Organized statement is not overlapping. It boxed, and can be nested. (see Part III point 3)
- Put normal case on “if” rather than “else”.  Eg: if (success) then bla else error_handle. Instead of: if (success != true) then error_handle else bla.
- About 50-70% use if and else instead if alone.  Eg: if bla then a = 2 else a =1   instead of: a = 1; if bla then a = 2.
- If you use “case”, ensure default case is detecting legitimate default value. Never tempt to use default as one case remaining

### B. Controlling Loops
- Treat code inside loop like a black box.
- Never use last value of loop as the result.  Eg: for (i = 0; i < 10; i++) { }; a = i / 2;
- Mentally check (either using simulation or calculator) for condition loop. (Part II - E. Point 2)
- Avoid one letter variable naming for loop (i, j, k). Try use more meaningful variable name.

### C. Unusual Control Structures
- Use multiple return to enhance readability. Multiple return is better than deeply nested “if”.

### D. Table Driven Methods
- Change multiple complicated logic checking using table.  Eg: if chars == 1 then bla; … if chars == 26 then blabla; -> return chars[char_at].  This chars is either hash or array prefilled with statement.
- Usually used when data have “stair” value, or grading value. Not totally different logic.

### E. General Control Issues
- Never emulate other type if you means boolean. Also it doesn’t necessary to test boolean with the value.  Eg: if is_ready == True.  Use: if is_ready -> for reducing mental mapping.
- Try to use positive boolean if needed.  Eg: if is_success blah, else blahblah.  Instead of: if not success blahblah, else blah.  If you really need negative one, try use: if error blahblah, else blah.
- Apply De Morgan theorem to simplify logic. a. not A and not B -> not ( A or B ) b. not A or not B -> not ( A and B )
- Use parentheses to ensure expression meaning, rather than relying on the evaluation order. Eg: if ((a < b) == (c == d)) Instead of: if ( a < b == c == d )
- Organize numeric test so they follow the points on a number line. Eg: MIN_ELEMENTS <= i and i <= MAX_ELEMENTS i is in the middle like this comparison: MIN_ELEMENTS <= i <= MAX_ELEMENTS in math Also this is mental mapping just like cartesian x. Which smaller value is on the left and larger value in on the right.
- To reduce nested logic block, we can consider a. Multiple flat logic b. Consider use case or elif(else if) instead nested if c. Refactor inside logic with routines d. Use polymorphism or pattern matching
- Measure complexity with McCabe technique: a. Start with 1 b. Add 1 every loop (for, while), branching (if, else if, case), comparison (and & or) The guideline is: a. 1-5 routine is fine b. 6-10 need to simplify the routine c. 10+ break part to new routine and call it on the first routine
- All complexity so far: naming, number of params, variable span, variable live, nested logic, and mc cabe


## Part V: Code Improvements

- Product defect can be detected better using code inspection (code review) instead of test. And Extreme programming (combination pairing, code inspection, code review, testing) can detect almost 90% - 97% product defect.
- Testing does not improve software qualities. Test results are an indicator of quality. If you want to improve your software, do not test more; develop better.
- Writing test first are have more benefit: a. We can detect defect earlier b. Force us to think at least a little bit about requirements and design before writing code c. Expose requirements problem sooner, because if it hard to test then requirements is poor.
- Test every branching, loop, and condition (Mc Cabe complexity). Using all true path first, and then evaluate every branching, looping, and condition to false one by one.
- For data testing, we should try combination of all branching. For example if there any 2 if else, then we should try all if true, one if false, opposite, and both if false. If it related to value, test edge case.
- Try to use case where hand calculation are enough. Using ugly number it's no more likely to than any other number in its equivalence class.
- Problematic class / routine usually tend to have more problem. Recent changed code also can introduce problem.
- Consider refactoring when: a. Code duplicate (when you need to change a piece of code in two places. b. Routine is too long. c. Loop is too deeply nested. d. Class has poor cohesion (also has too much coupling). e. Routine has too many params
- Putting busiest loop at inside loop can improve performance


## Part VI: System Considerations

- More programmer tend to more communication. 2 programmer -> 1 communication, 4 programmer -> 6 communication, 10 programmer -> 45 communication. n (n - 1) / 2
- When size increase (for the example increase twice). The effort is not increase twice. It can increase more than linear. (Communication, Planning, Management, Requirements development, System functional design, Interface design and specification, Architecture, Integration, Defect removal, System testing, Document production)
- Estimating project usually failed, either it is late by factor two, or over budget by 100%. The better is estimating while project progressing. And if project late, usually it will be more late on the end of schedule.


## Part VII: Software Craftsmanship

- Use whitespace to enhance readability.
- Self preferences, for breaking multiple statement, ensure the operand is on the front Eg: if a + 1 == 0 \          && b == 10 \          || c == 5: Reason is: Easier to check what operation performed
- Preferable to new line params if params long. Eg:  def routine(     args_1,     args_2,     args_3   ): Instead of: def routine(args_1,                    args_2,                    args_3): Reason is when “routine” name changed, we have to reformat the arguments again.
- Code that need to comment, is a bad code, unless the comment is for documentation purpose only, or clearing intent for the routine.
- It's easy to confuse motion with progress, busy-ness with being productive. The most important work in effective programming is thinking, and people tend not to look busy when they're thinking. If I worked with a programmer who looked busy all the time, I'd assume that he was not a good programmer because he wasn't using his most valuable tool, his brain.
- Tricky code means bad code. Class who have error more than average tend to had more errors. Think to rewrite it.
- If you working on repetitious code, hard to test, cannot reuse the class. It means that the class have tight coupling.
- If it hard to you, then it is harder for next programmer.
