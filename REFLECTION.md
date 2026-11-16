# Reflection

## 1. TODO 1-19 and complexity

TODO 1 allocates exactly one pointer slot for each child and initializes every slot to nullptr. An ending allocates no child array. Its complexity is O(c), where c is the number of choices in that scene.

TODO 2 deletes every child scene, the child-pointer array, and the scene's data. Because the destructor recursively deletes the entire subtree, its complexity is O(n), where n is the number of scenes in that subtree.

TODO 3 checks whether a scene has zero choices. It is O(1).

TODO 4 checks whether an index is valid and returns the corresponding child. It is O(1).

TODO 5 checks whether an index is valid and stores a child pointer in that slot. It is O(1).

TODO 6 recursively counts the current scene and every scene below it. It is O(n), where n is the number of scenes.

TODO 7 recursively counts the scenes that are endings. It visits every scene once, so it is O(n).

TODO 8 recursively finds the greatest height among the children and adds one. It is O(n).

TODO 9 deletes the root and sets root_ to nullptr. The Scene destructor then deletes the whole tree, so it is O(n).

TODO 10 opens the story file, reads START_HP, loads the root scene, and stores it in the tree. It processes the file once, so it is O(n).

TODO 11 reads one scene and recursively reads all of its children in pre-order. Every scene is processed once, so it is O(n).

TODO 12 repeatedly reads input until the user enters a valid choice. One valid attempt is O(1), not counting the number of invalid attempts.

TODO 13 follows one path through the story, updates HP and loot, and searches the inventory for duplicate items. Its complexity is O(hb), where h is the path length and b is the inventory size.

TODO 14 prints the scene count, ending count, and height. Each statistic traverses the tree, so the overall complexity is O(n).

TODO 15 starts the recursive spoiler traversal. It is O(n).

TODO 16 recursively visits every scene and prints each ending with its path. It is O(n).

TODO 17 follows one path and compares at most three immediate child choices at each scene. Its complexity is O(h), which is O(n) in the worst case.

TODO 18 recursively examines every possible subtree and returns the best surviving loot result. Since every scene is visited once, it is O(n).

TODO 19 calls TODO 18 and prints the optimal result. Its complexity is O(n).

## 2. Exact-size child allocation

Each scene allocates exactly numChoices_ pointer slots because it only needs enough space for its actual children. This avoids wasting space on unused slots and follows the required Scene<T>** representation. An ending has zero choices, so choices_ remains nullptr and no child array is allocated.

## 3. Scene destructor

The destructor first deletes every child scene in the choices array. Each child destructor recursively deletes its own children. After the children are deleted, the destructor deletes the choices_ array and then deletes data_. If choices_ were deleted first, the program would lose the pointers to the child scenes and would leak their memory.

## 4. Greedy versus optimal in my story

My story is about Nova delivering a package through the Clockwork Valley.

The greedy advisor takes path 1-1. It chooses the glittering shortcut because the next scene gives 7 loot immediately. The following scene contains a clockwork dragon, causing Nova to lose 15 HP and die. The greedy run ends with 7 loot.

The optimal advisor takes path 2-1-1-1-1-1. This path follows the old road, climbs toward the bridge, crosses the bridge, enters the tower, climbs to the summit, and opens the vault. It reaches an ending with 13 total loot and surviving HP.

Greedy loses because it only looks at the loot in the next scene. It does not consider that the shortcut leads to a scene that kills Nova. The optimal advisor considers the complete paths and chooses a path that keeps Nova alive while collecting more loot.

## 5. Why bestLoot is O(n)

bestLoot is recursive, but it is not exponential in this program because the story is a tree and every scene has only one parent. The function visits each scene and checks each of its children once. Although there are multiple complete paths, paths share scenes above their branches, and each scene is processed only during its one recursive call. Therefore, with n total scenes, bestLoot takes O(n) time.

## 6. List methods used

The game uses search to check whether Nova already owns an item. If the item is not already in the inventory, addFront adds a new Item pointer to the list. The size method prints the number of items, and print displays each item at the end of the game. The List destructor uses deleteFront to clean up the dynamically allocated items.

A search through a linked list containing b items costs O(b) in the worst case because it may need to examine every item.