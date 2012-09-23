--- 
Status: implemented
--- 

nanoc allocates a lot of memory, which makes the garbage collections take a long time (1/4s or more). Since the garbage collections are quite frequent, this makes nanoc spend a lot of time garbage collecting. Real-world tests with large sites indicate that occasionally, 75% of time is spent garbage collecting!
