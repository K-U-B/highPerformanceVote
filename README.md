# highPerformanceVote
High performance/efficiency variant of a scalable voting system for appEngine, written in C.

## Design
Differences to simpleVote, which is written in PHP on top of Apache with a MySQL-Hierarchy for appEngine:

  * Self written frontend application server for communication with the voting app
    * FrontEnd-Server written form scratch in C
    * Using binary UDP messages for communication with mobile app
      * one UDP request -> two UPD answers: immediate ack, later real ack for upstream save of vote
      
    * target performance min. 10k votes/sec (with multithreading) -> 50-100ms aggregation window
    * communicate bucket upstream to a minimum of three backend servers via TCP
    * In a later step
      * multithreading (openMP Tasking or pthreads)
      * ? message authentication (different levels: CRC, MD5/SHA...)
      * ? message encryption (different levels: AES or {ec}RSA ...)
      
  * Self written backend server hierarchy using binary protocol via TCP for communication with the frontend servers
    * target performance min. 100 frontend servers -> 1000 frontend messages -> min. 1 million request / sec 
    * min. 3 (av. zone) per Region (0.1 sec latency) on spine level
    * root level one per region on min. 3 regions (latency 1 sec)
    
      -> up to 100 spine server (up to 10k front end server) -> up to 100 million votes / sec world wide.
    * root hierarchy to read out (replicate) vote results in about less than 3 secs from vote 
    
