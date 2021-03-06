# Dynamic allocation 
A dynamic allocation mechanism is a single Python file which is used in the "dynamic track" of The Choice Engineering Competition. For more details on the competition, see [the competition's website](http://decision-making-lab.com/competition/index.html). 

 ## What is a dynamic allocation model?

 A dynamic allocation model is used to determine the rewards in a single trial for a single subject, participating in the experiment of *The Choice Engineering Competition*. The allocation mechanism should indicate whether or not to allocate a reward for either of the experiment’s two alternatives. The rewards available for allocation are binary (either 1 or 0) and are constrained such that during the 100 trials of the experiment, each of the alternatives should be associated with exactly 25 rewards (i.e. 1's, and 75 should be associated with 0's). The input to the dynamic allocation model is current experiment's history, namely previous allocations and respective choices, and its output is the allocation of rewards for the next trial.

 The goal of the competition, and hence of the dynamic allocation model, is to generate an allocation mechanism that maximizes the choices of humans in one specific alternative, termed the "target alternative". In the actual experiments the target alternative will be placed randomly either on the left or the right part of the subject's screen. However, in the output of the allocation model, the target alternative should be always placed first (see below). 

## How should the dynamic allocation file be used?

 In the course of an experiment, your dynamic allocation model would be called repeatedly, once per every trial, and the allocation indicated by your algorithm will be revealed to the participant. Note that the paradigm used in the competition is that of “partial feedback”, by which only the reward associated with the chosen alternative is revealed (the reward associated with the unchosen alternative is not). Hence, only one of the rewards output by the dynamic allocation mechanism is actually revealed to the subject, according to her choice.

### Receiving input
Your model will be called with __three command-line arguments__ that you may parse (e.g. with [sys.argv](https://docs.python.org/3.7/library/sys.htmlsys.argv)):
1. The first input is a list of __previous allocations to the target alternative__. Each entry in the list is either 1, indicating that in that index a reward was allocated, or 0, indicating that it was not. For example, the list [1, 0, 0, 0] received as the first input for your program indicates that the experiment is currently at its 5'th trial, that on the first trial a reward was allocated to the target alternative and that no rewards were allocated in trials 2, 3, and 4.
2. The second input is in the same format as the first, but it indicates __previous allocations to the second ("anti-target") alternative__. Hence, for example, the list [0, 1, 1] as the second input to your program indicates that the game is currently at its 4'th trial, that on the first trial a reward was not allocated to the anti-target alternative and that on the second and third trial rewards were allocated to that alternative.
3. The third input is a list of __previous choices__, where 1's indicate a choice in the target alternative and 0's indicate choice in the anti-target alternative. For example, the list [1, 1, 1, 0, 0, 0] received as the third input indicates that the experiment is currently at its 7'th trial, that on the first three trials the target side was chosen by the subject and that on the last three trials it was not. 
 
### Providing output
Your model should indicate the allocation of rewards for the next trial by printing (to the standard sys.stdout, e.g. using [print]( https://docs.python.org/3/tutorial/inputoutput.html)) a single string in the format of "(T, N)", where both T and N are either the character 1 or 0, T represents the allocation to the target side and N the allocation the non-target side. 

Hence, __your model's output should be one of the following four__ strings:
1. "(0, 0)"
2. "(0, 1)"
3. "(1, 0)"
4. "(1, 1)"

### Code examples and templates 
This folder contains a [template of code](https://github.com/ohaddan/competition/blob/master/templates/dynamic_template.py) you may use to write your dynamic mechanism. In addition examples for mechanisms may also be found in this folder:

* [WSLS dynamic mechanism](https://github.com/ohaddan/competition/blob/master/templates/dynamic_wsls.py) - an example of a dynamic mechanism which tries to optimize the bias of subjects, assuming they follow the principle of [Win–stay, lose–switch](https://en.wikipedia.org/wiki/Win%E2%80%93stay,_lose%E2%80%93switch).
