**Status: Pending**

The compilation process should be fairly easy parallellizable. Even though there are dependencies between items, layouts, … the compiler can currently handle this rather well. Parallellizing nanoc would make the compilation process faster, and would likely lead to a cleaner API. (For example, no more compilation stack.)

This will likely not be implemented because it is very low-priority; after all, the major Ruby implementations have  a global interpreter lock (GIL) that prevent true parallellism.
