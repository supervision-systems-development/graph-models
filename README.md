# graph-models

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

  



  
  

  


