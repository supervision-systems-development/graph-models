# General Functionality

This is a supervision system capable of real-time supervision as well as general analysis of task execution completeness based on a task model represented as a dependency graph with node functions. 

This system utilises rewriting rules for action analysis. Every time a new action is sent to the input, the system configuration is rewritten and the result shows the immediate feedback. After the completion of the task, the final configuration shows additional information such as whether the task has been executed correctly. 

Actions can be sent to the supervision system either one-by-one (for example, in real time) or as a list of actions (for example, of an already completed task) for analysis. 

The 'SupervisionSystem.maude' file contains several examples at the end of the file.


# Usage

The supervision system can be given inputs either as commands once it has been loaded to maude, or the commands can be appended at the end of the file in order to be executed once the file is loaded.

## Existing list of actions

In order to perform analysis of an already completed task, the command 
`rew List_Of_Actions | nil | nil | nil | Task_Pattern | starting .` 
can be used, with the list of actions in place of of `List_Of_Actions` and with the task model in place of `Task_Pattern`. 

![The pattern used to show an example of a trace of actions in the paper.](https://github.com/supervision-systems-development/graph-models/blob/main/figures/toy_example.png)

For example, the pattern and the sequence of actions used in the paper to show an example of the trace is can be used as an input:
`rew (a(-5) ; a(1) ; a(3) ; a(2) ; a(4) ; a(4) ; a(5) ; a(-7)) | nil | nil | nil | (n[4]: nseq) -> (n[5]: nor) ;; (n[5]: nor) -> (n[-7]: nseq) ;; (n[2]: nseq) -> (n[5]: nor) ;; (n[3]: nseq -> n[4]: nseq) ;; (n[1]: nxor -> n[2]: nseq) ;; (n[1]: nxor -> n[3]: nseq) ;; (n[-5]: nseq -> n[1]: nxor) | starting .
` 
which results in the following output:

![rewrites: 1721 in 0ms cpu (0ms real) (~ rewrites/second)                                                                                                                      result Config: nil | t(-5,1),t(3,1),t(1,-1),t(2,-1),t(4,1),t(5,1),t(-7,1) | a(-5) ; a(1) ; a(3) ; a(4) ; a(5) ; a(-7) | a(2) ; a(4) | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor      -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;; n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | correct     ](https://github.com/supervision-systems-development/graph-models/blob/main/figures/github_example.png)

This is an example of an already completed task being anaylsed after it has been completed. The result is in the form of a configuration containing elements of the configuration separated by `|`. The first element `nil` indicates, that the list of input actions is empty. The second element shows the list of indicators. The third element shows the list of correct actions, implying that this is the subset of the input list that conforms to the pattern and that is correct. The fourth element shows the list of incorrect actions, indicating that these were the mistakes made during the execution. The Fith element of the list shows the pattern and the sixth element indicates that the task has been completed correctly with a `correct` statement. 

## Conformance Checking

The approach described above can be used for conformance checking, however, as in conformance checking only the final conifugrations that resulted in a correct status, if they exist, are relevant, a separate command can be used to match a pattern containing the 'correct' status indicating the conformance of the sequence of actions to the pattern. 

An example of such command matchin a pattern containing the 'correct' status for the example pattern shown above and the example sequence of actions used in the paper: 

` search (a(-5) ; a(1) ; a(3) ; a(2) ; a(4) ; a(4) ; a(5) ; a(-7)) | nil | nil | nil | (n[4]: nseq) -> (n[5]: nor) ;; (n[5]: nor) -> (n[-7]: nseq) ;; (n[2]: nseq) -> (n[5]: nor) ;; (n[3]: nseq -> n[4]: nseq) ;; (n[1]: nxor -> n[2]: nseq) ;; (n[1]: nxor -> n[3]: nseq) ;; (n[-5]: nseq -> n[1]: nxor) | starting =>* List_Of_Input_Actions | List_Of_Indicators | List_Of_Correct_Actions | List_Of_Incorrect_Actions | Pattern | correct . `

This command will only show a solution if the sequence conforms to a pattern, which will be indicated with a 'correct' status after the entire input sequence has been executed. 

## Real-time supervision feedback

In order to perform real-time supervision with feedback after each action, each action has to be sent to the input separately, as it is being executed, which is logical, as during the execution the supervision system does not know, which actions the student will execute in the future. 

Real-time feedback during supervision after each student action implies the provision of each student action to the input of the system configuration. After the input of the student action, **one** rewriting rule is applied to the system and the resulting configuration contains the feedback. The correctness of incorrectness of the action is determined by the list of actions that the action is added to. In case the action has been correct, it is added to the list of correct actions, and in case it has been incorrect, it is added to the list of incorrect actions. In order to determine, which list the action has been added to, the lists of correct and incorrect actions from the previous configuration can be compared with the lists of correct and incorrect actions from the resulting configuration. Additionally, the resulting configuration will show, whether the sequence of actions has resulted in a correct execution of the task represented by the pattern. 

An example of real-time supervision is shown using the example pattern used in the paper and shown on a figure above and the example sequence of actions used in the paper with the example pattern. The example sequence contains following actions: -5, 1, 3, 2, 4, 4, 5, -7 . Actions -5 and -7 are artificial and represent actions S and F, which are required for the system to know when the execution has finished. Corresponding nodes to actions S (-5) and F (-7) are present in the pattern. 

As the feedback is given in the subsequent configuration after the application of one rule, there are two approaches towards applying a rewriting rule exactly once: using the `rew [1]` command or using the `search` command with the `=>1` argument after the input configuration followed by a configuration with all variables specifying that only one rule should be executed. The 'search' command lists the values of the variables while the `rew [1]` command shows the rewritten configuration, which is more convenient, which is why `rew [1]` will be used to show the real-time feedback in this example. 

The first, initial action is `-5`. It is sent to the input of the system in the first position of the configuration as a list containing a single action – `-5`: ` rew [1] (a(-5)) | nil | nil | nil | (n[4]: nseq) -> (n[5]: nor) ;; (n[5]: nor) -> (n[-7]: nseq) ;; (n[2]: nseq) -> (n[5]: nor) ;; (n[3]: nseq -> n[4]: nseq) ;; (n[1]: nxor -> n[2]: nseq) ;; (n[1]: nxor -> n[3]: nseq) ;; (n[-5]: nseq -> n[1]: nxor) | starting . `. The result of applying one rule is shown in the output following immediately: 

[rewrite [1] in GRAPH-RL : a(-5) | nil | nil | nil | (((((n[1]: nxor -> n[3]: nseq ;; n[-5]: nseq -> n[1]: nxor) ;; n[1]: nxor -> n[2]: nseq) ;; n[3]: nseq ->
    n[4]: nseq) ;; n[2]: nseq -> n[5]: nor) ;; n[5]: nor -> n[-7]: nseq) ;; n[4]: nseq -> n[5]: nor | starting .
rewrites: 10 in 0ms cpu (0ms real) (10000000 rewrites/second)
result Config: nil | t(-5,1) | a(-5) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;;
    n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing]()

The rewritten rule ` nil | t(-5,1) | a(-5) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;;
    n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing ` shows that the list of input actions is empty – which is indicated with a 'nil', and the list of correct actions has had the action from the input list added to it, and the list of correct actions shows a clear diefference compared to the previous configuration, in which the list of correct actions was empty, indicated by a 'nil'. An indicator for the action '-5' seems to have been added to the list of indicators, indicating the indicator '1' for the action '-5', which shows that it has been activated. The status has changed to 'executing' after the starting action `-5` has been executed, which means that the system is actively receiving user actions. 

In order to send the next action to the supervision system, the next action needs to be added to the list of input actions of the configuration resulting from the analysis of the previous action (the most recent configuration). In this case, the configuration ` nil | t(-5,1) | a(-5) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;;
    n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing ` is the configuration that resulteed after the input of the previous action. This configuration needs to have the next action, `1`, added in the first position, in the place of the list of input actions, which is empty ('nil') in the configuration. This is done by placing the next action instead of `nil`, as shown: ` a(1) | t(-5,1) | a(-5) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;;
    n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing `. This is the new configuration with the next action that can be executed using the same command ` rew [1] a(1) | t(-5,1) | a(-5) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq -> n[4]: nseq ;;
    n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing . `, which can be sent to the input of the Maude System, which will offer an immediate result: 
[rewrite [1] in GRAPH-RL : a(1) | t(-5,1) | a(-5) | nil | (((((n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor) ;; n[4]: nseq -> n[5]: nor) ;; n[3]:
    nseq -> n[4]: nseq) ;; n[2]: nseq -> n[5]: nor) ;; n[1]: nxor -> n[3]: nseq) ;; n[1]: nxor -> n[2]: nseq | executing .
rewrites: 135 in 0ms cpu (0ms real) (470383 rewrites/second)
result Config: nil | t(-5,1),t(1,1) | a(-5) ; a(1) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq ->
    n[4]: nseq ;; n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing]()

which immediately shows the resulting configuration ` nil | t(-5,1),t(1,1) | a(-5) ; a(1) | nil | n[1]: nxor -> n[2]: nseq ;; n[1]: nxor -> n[3]: nseq ;; n[2]: nseq -> n[5]: nor ;; n[3]: nseq ->
    n[4]: nseq ;; n[4]: nseq -> n[5]: nor ;; n[5]: nor -> n[-7]: nseq ;; n[-5]: nseq -> n[1]: nxor | executing ` offering feedback immediately after the action has been sent to the input, taking into account past actions. In case of this configuration, the action `1` has been added to the list of correct actions, which now contains `-5` and `1`.








# Sorts:

  Graph:       a set of arcs
  
  Arc:         two nodes connected with a directional vertex
  
  Node:        a graph node with a unique id with a function (start, fin, oth) and a type (nseq, nand, nor, nxor)

  Action:      an action that indicates the unique id of a node, but does not indicate its function or type

  ActionList:  a list of actions

  Tuple:       a tuple with two Int values, used to represent cookies

  TupleList:   a list of tuples


# Functions:

  first : Tuple -> Int
  
  Used to get the first value of the tuple

  firstAction : ActionList -> Action 
  
  Used to get the first action from a list of actions

  firstActionInt : ActionList -> Int 

  Used to get the Int value of the first action from a list

  checkStatus : CookieList ActionList Graph -> Bool

  Used to check whether the sequence has been successfully completed by checking if the last node had a "fin" function. Uses the cookie sort, not actively used in current implementation.

  checkType : Action Graph -> String

  Used to check the type of the node by action – essentially retrieves the node type from the graph by ID using the action ID, which matches the node ID.

  isStartingNode : Action Graph -> Bool

  Used to check whether the function of the element at the start of the sequence is "starting"

  checkFunc : Action Graph -> String

  Used to check the function of a node beyond the start of the execution

  isAnd : Action Graph -> Bool

  Checks if the node type is "and"

  isOr : Action Graph -> Bool

  Checks if the node type is "or"

  isXor : Action Graph -> Bool

  Checks if the node type is "xor"

  isSeq : Action Graph -> Bool

  Checks if node type is "seq", meaning sequential

  consume : CookieList CookieList -> CookieList

  Removes (consumes) cookies from cookielist, not actively used in current implementation.

  add : ActionList ActionList -> ActionList

  Adds elements from one list to another, avoiding duplicates. Not actively used in current implemetation. 

  checkParentsInList : Action ActionList ActionList Graph -> String

  Used to check how many parent nodes are represented in the cookies. Used in previous implementation of cookies, not actively used in current implementation.

  allActionsListInList : ActionList ActionList -> Bool

  Used to check whether all actions of the first actionlist are present in the second actionlist.

  someActionsListInList : ActionList ActionList -> Bool

  Used to check whether some actions of the first actionlist are present in the second actionlist.

  oneActionListInList : ActionList ActionList -> Bool

  Used to check whether one action from the first actionlist is present in the second actionlist.

  parents : Action Graph -> ActionList

  Retrieves all parent nodes and represents them as actions (without function and type) by the child node and the graph.

  children : Action Graph -> ActionList

  Used to get a list of all children of a node represented by the node ID from the action within a graph. 

  listChildren : ActionList Graph -> ActionList

  Returns a list of all children from a lsit of actions representing a list of nodes. Used for the graph traversal.

  _in_ : Action ActionList -> Bool

  Checks if an action is in an actionlist. 

  tupleInTupleList : Tuple TupleList -> Bool

  Checks if a tuple is inside a tuplelist. Used for cookies.

  removeTupleFromTupleList : Tuple TupleList -> TupleList

  Removes a tuple from a list of tuples. 

  activateCookie : Int TupleList -> TupleList

  Activates a cookies, creates a tuple representing that cookie node ID and adds it to the existing list. 

  isCookieActive : Int TupleList -> Bool

  Checks if a cookie is active in a given list by node ID.

  deactivateCookie : Int TupleList -> TupleList

  Deactivates a cookie, if cookie is active, in a tuple list and returns tuple list. 

  deactivateCookieList : TupleList TupleList -> TupleList

  Deactivates a list of cookies (tuples), if they ar epresent, in a list of tuples.

  deactivateActionCookies : ActionList TupleList -> TupleList

  Deactivates cookies represented by a list of actions in a list of cookies represented by a tuplelist. Essentially removes all tuples from the tuplelist if they are in the actionlist. 

  _in_ : Cookie CookieList -> Bool

  Used ot check whether cookies is inside a cookielist. Not used in current implementation.

  isArc : Node Node Graph -> Bool

  Checks, if there is a vortex between first and second node ina graph. Not used in current implementation.

  actionListSubtraction : ActionList ActionList -> ActionList

  Removes second list from first list of actions, if present. 

  tupleListIntersection : TupleList TupleList -> TupleList

  Returns the set of tuples present in both tuplelists.

  tupleSubtraction : TupleList TupleList -> TupleList

  Removes second list of tuples from first list of tuples, if present. 

  BFSLoopDetect : Action Action ActionList ActionList Graph -> Bool

  Breadth first search based loop detection algorithm. Checks if the previous (current) node can be reached from the starting node before the subsequent (next) node. 

  DFSSubgraphSearch : Action Action Graph ActionList -> ActionList

  Depth first search based subgraph search algorithm. Finds all subgraphs from one node to another. 

  checkIfCorrectNode : Action Action ActionList Graph ActionList -> ActionList

  Helper function for the  DFSSubgraphSearch algorithm. 

  removeDuplicates : ActionList -> ActionList

  Removes duplicates from an actionlist. 

  removeDuplicatesHelper : ActionList ActionList -> ActionList

  Helper function for duplicate removal function.

  getAllPathsBetween : Action Action Graph -> ActionList

  Gets all paths between two nodes. Wrapper function for the DFSSubgraphSearch.

  getAllPathsForLoopDeactivation : Action Action Graph -> ActionList

  Wrapper function for the getAllPathsBetween and DFSSubgraphSearch functions, returns a list of actions ready for deactivation.

  loopFilter : Action Action Graph TupleList -> TupleList

  Function intended to filter each cookie list (represented as tuple list) based on presence of loops. Abandoned in current implementation due to high complexity.

  detectLoop : Action Action Graph -> Bool

  Wrapper function for BFSLoopDetect, assumes starting node is 0(!!!), checks if a loop is detected before the next transition. 

  lastAction : ActionList -> Action

  Returns last action from an actionlist.

  isCookieNotActive : Int TupleList -> Bool

  Checks if cookie is NOT active. 

  allParentCookiesActive : Action TupleList Graph -> Bool

  Checks if all parent cookies are active. 

  allParentCookiesActiveHelper : ActionList TupleList Graph -> Bool

  Helper function for allParentCookiesActive.

  atLeastOneParentCookieActive : Action TupleList Graph -> Bool

  Checks if at least one parent node has an active cookie. 

  atLeastOneParentCookieActiveHelper : ActionList TupleList Graph -> Bool

  Helper function for atLeastOneParentCookieActive.

  xorParentCookieActive : Action TupleList Graph -> Bool

  Checks if the configuration of parent node cookies satisfies the built-in XOR function. 

  xorParentCookieActiveHelper : ActionList Bool TupleList Graph

  Helper function for xorParentCookieActive. Uses a first action cookie as an exit condition. Might be an incorrect approach. 

  



  
  

  


