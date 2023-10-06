### Find

1 0xc0000008

2 0x80488ee

3 
```js
080488e8 <_start>:
 80488e8:       55                      push   ebp
 80488e9:       89 e5                   mov    ebp,esp
 80488eb:       83 ec 18                sub    esp,0x18
 80488ee:       8b 45 0c                mov    eax,DWORD PTR [ebp+0xc]
 80488f1:       89 44 24 04             mov    DWORD PTR [esp+0x4],eax
 80488f5:       8b 45 08                mov    eax,DWORD PTR [ebp+0x8]
 80488f8:       89 04 24                mov    DWORD PTR [esp],eax
 80488fb:       e8 94 f7 ff ff          call   8048094 <main>
 8048900:       89 04 24                mov    DWORD PTR [esp],eax
 8048903:       e8 d3 21 00 00          call   804aadb <exit>
```
The instruction is `mov    eax,DWORD PTR [ebp+0xc]`. 

4
```
void _start(int argc, char* argv[]) { exit(main(argc, argv)); }
```
explain:

    store the EP

    move current SP to EP as a fixed pointer

    allocate 0x18 space to local variables

    move [BP+0xc] to AX

    move AX to [SP+0x8]

    move [BP+0x8] to AX

    move AX to SP

    call main fuction

    after main move AX to the space SP pointed

    call exit function

  

5 Because it try to store some value in stack like local variable.

  
  

### Step

  

1
Current thread: {Name: "main", Address: 0xc000edbc}

Others: `{tid = 2, status = THREAD_BLOCKED, name = "idle", '\000' <repeats 11 times>, stack = 0xc0104f14 "", priority = 0, allelem = {prev = 0xc000e020, next = 0xc0039da0 <all_list+8>}, elem = {prev = 0xc0039d88 <fifo_ready_list>, next = 0xc0039d90 <fifo_ready_list+8>}, pcb = 0x0, magic = 3446325067}`
  

2 Backtrace :
```js
process_execute (file_name=0xc0007d50 "do-nothing") at ../../userprog/process.c:57
#1  0xc0020a19 in run_task (argv=0xc0039c8c <argv+12>) at ../../threads/init.c:315
#2  0xc0020b57 in run_actions (argv=0xc0039c8c <argv+12>) at ../../threads/init.c:388
#3  0xc00203d9 in main () at ../../threads/init.c:136
```


3
Current: {Name: "do-nothing\000\000\000\000\000", Address: 0xc010bfd4}

Others: 
`{tid = 1, status = THREAD_BLOCKED, name = "main", '\000' <repeats 11times>, stack = 0xc000ee7c "", priority = 31, allelem = {prev = 0xc0039d98 <all_list>, next = 0xc0104020}, elem = {prev =0xc003b7b8<temporary+4>, next = 0xc003b7c0 <temporary+12>}, pcb = 0xc010500c, magic =3446325067}`

`{tid = 2, status = THREAD_BLOCKED, name = "idle", '\000' <repeats 11 times>, stack = 0xc0104f14 "", priority = 0, allelem = {prev = 0xc000e020, next = 0xc010b020}, elem = {prev = 0xc0039d88 <fifo_ready_list>, next = 0xc0039d90 <fifo_ready_list+8>}, pcb = 0x0, magic = 3446325067}`

4 It create `new_pcb` , use `struct process* new_pcb = malloc(sizeof(struct process));`

5 `{edi = 0x0, esi = 0x0, ebp = 0x0, esp_dummy = 0x0, ebx = 0x0, edx = 0x0, ecx = 0x0, eax = 0x0, gs = 0x23, fs = 0x23, es = 0x23, ds = 0x23, vec_no = 0x0, error_code = 0x0, frame_pointer = 0x0, eip = 0x80488e8, cs = 0x1b, eflags = 0x202, esp = 0xc0000000, ss = 0x23}`

6 
Because it restore the RGs value it save and doing this will let the pointer(eip, esp) point to user space memory.


7
sp is different.
```js
eax            0x0      0
ecx            0x0      0
edx            0x0      0
ebx            0x0      0
esp            0xc0000000       0xc0000000
ebp            0x0      0x0
esi            0x0      0
edi            0x0      0
eip            0x80488e8        0x80488e8
eflags         0x202    [ IF ]
cs             0x1b     27
ss             0x23     35
ds             0x23     35
es             0x23     35
fs             0x23     35
gs             0x23     35
```

8
```js
#0  _start (argc=-268370093, argv=0xf000ff53) at ../../lib/user/entry.c:6
#1  0xf000ff53 in ?? ()
```


### Debug

1 
Because before crash it will sub esp 0x18, and we assign sp 0. So i modify it to assign esp 0x18.

2 
It should return 12.

3
