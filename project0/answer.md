# answer

## First

1. 0xc0000008
2. 0x80488ee
3. <_start> 
    The code is:
    ```
    080488e8 <_start>:
    80488e8:       55                      push   %ebp
    80488e9:       89 e5                   mov    %esp,%ebp
    80488eb:       83 ec 18                sub    $0x18,%esp
    80488ee:       8b 45 0c                mov    0xc(%ebp),%eax
    80488f1:       89 44 24 04             mov    %eax,0x4(%esp)
    80488f5:       8b 45 08                mov    0x8(%ebp),%eax
    80488f8:       89 04 24                mov    %eax,(%esp)
    80488fb:       e8 94 f7 ff ff          call   8048094 <main>
    8048900:       89 04 24                mov    %eax,(%esp)
    8048903:       e8 d3 21 00 00          call   804aadb <exit>
    ```
    The instruction is: `mov    0xc(%ebp),%eax`
4.
    ```
    void _start(int argc, char* argv[]);
    void _start(int argc, char* argv[]) { exit(main(argc, argv)); }
    ```
    **explain:**
    store the EP 
    move current SP to EP as a fixed pointer
    allocate 0x18 space to local variables
    move [BP+0xc] to AX
    move AX to [SP+0x8]
    move [BP+0x8] to AX
    move AX to SP
    call main fuction
    after main move AX to SP
    call exxit function

5. Because it want to move last function's local variable to this but that variable is already free.


## Second

1. Current running: 0xc000e000 {tid = 1, status = THREAD_RUNNING, name = "main"}
    others: 0xc0104000 {tid = 2, status = THREAD_BLOCKED, name = "idle", '\000' <repeats 11 times>, stack
= 0xc0104f14 "", priority = 0, allelem = {prev = 0xc000e020, next = 0xc0039da0 <all_list+8>}, elem = {prev = 0xc0039d88 <fifo_ready_list>, next = 0xc0039d90 <fifo_ready_list+8>}, pcb = 0x0, magic = 3446325067}

2. Backtrace: 
    #0  run_task (argv=0xc0039c8c <argv+12>) at../../threads/init.c:310
    #1  0xc0020b57 in run_actions (argv=0xc0039c8c <argv+12>) at ../../threads/init.c:388
    #2  0xc00203d9 in main () at ../../threads/init.c:136






