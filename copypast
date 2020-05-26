section .text

global _start

;========================================================================================
;Expects: end of stack is the ascii to print, rcx - length of bytes to print
;
;Returns: none
;
;Destroys: rax, rcx, rdx, rdi, rsi
;========================================================================================
%macro PRINT 0
                mov     rdx, rcx
                mov     rax, 0x01
                mov     rdi, 1

                mov     rsi, rsp
                add     rsp, rcx
                syscall
%endmacro

;========================================================================================
;Expects: ah - ascii to print
;
;Returns: none
;
;Destroys: rax
;========================================================================================
ascii:
                push    rcx
                push    rdx

                push    ax
                inc     sp
                mov     rcx, 1

                PRINT

                pop     rdx
                pop     rcx

                ret
;===============================================================================
;Expects: rax - offset to string
;
;Returns: rdx - string length
;
;Destroys: none
;=================================================================================
strlen:
                push    rax
                xor     rdx, rdx

.rep:
                cmp     byte [rax], '$'
                je     .finish

                inc     rax
                inc     rdx
                jmp     .rep

.finish:
                pop     rax

                ret

;========================================================================================
;Expects: rax - offset string to print
;
;Returns: none
;
;Destroys: rax, rdi, rsi
;========================================================================================
string:
                push    rcx
                push    rdx

                call    strlen

                mov     rsi, rax
                mov     rax, 0x01
                mov     rdi, 1
                syscall

                pop     rdx
                pop     rcx

                ret
;=========================================================================================
;Expects: rax - number to print
;
;Returns: none
;
;Destroys: rax, rbx, rsi, rdi
;========================================================================================
bin:
                push    rcx
                push    rdx

                xor     rcx, rcx
                xor     bx, bx
                
.add:
                shr     rax, 1
                jc      .print1

                mov     bh, '0'
                jmp     .continue

.print1:
                mov     bh, '1'
        
.continue:
                inc     rcx
                push    bx
                inc     sp

                cmp     rax, 0
                jne     .add

                PRINT

                pop     rdx
                pop     rcx

                ret
;================================================================================================
;Expects: rax - number to print
;
;Returns: none
;
;Destroys: rax, rbx, rsi, rdi
;================================================================================================
oct:
                push    rcx
                push    rdx

                xor     rcx, rcx

.rep:
                mov     rbx, 111b
                and     rbx, rax

                shr     rax, 3

                add     bx, '0'
                shl     bx, 8
                push    bx

                inc     rcx
                inc     sp

                cmp     rax, 0
                jne     .rep

                PRINT

                pop     rdx
                pop     rcx

                ret
;================================================================================================
;Expects: rax - number to print
;
;Returns: none
;
;Destroys: rax, rbx, rsi, rdi
;================================================================================================
hex:
                push    rcx
                push    rdx

                xor     rcx, rcx
                xor     rbx, rbx

.rephex:
                mov     rbx, 1111b
                and     rbx, rax

                shr     rax, 4
                shl     bx, 8

                cmp     bh, 10
                jb      .number
                
                add     bh, 'A' - 10d
                jmp     .stackpush

.number:
                add     bh, '0'

.stackpush:
                push    bx
                inc     sp
                inc     rcx

                cmp     rax, 0
                jne     .rephex

                PRINT

                pop     rdx
                pop     rcx
                ret
                ;================================================================================================
;Expects: rax - number to print
;
;Returns: none
;
;Destroys: rax, rbx, rsi, rdi
;================================================================================================
dec:
                push    rcx
                push    rdx

                xor     rcx, rcx
                xor     rdx, rdx

.repdec:
                mov     rbx, 10d
                div     rbx
                
                mov     rbx, rdx
                
                shl     bx, 8
                add     bh, '0'

                push    bx
                inc     sp
                inc     rcx

                xor     rdx, rdx

                cmp     rax, 0
                jne     .repdec

                PRINT

                pop     rdx
                pop     rcx

                ret
;==================================================================================================
;Expects: rdx - offset to format string
;
;Returns: none
;
;Destroys: rax, rbx, rcx, rdx, rdi, rsi
;==================================================================================================
printf:
                pop     rcx                

.next:
                cmp     byte [rdx], '$'
                je      .end

                cmp     byte [rdx], '%'
                je      .string

                mov     ah, byte [rdx]
                call    ascii
                
                jmp     .return

.string:
                inc     rdx

                cmp     byte [rdx], 's'
                jne     .percent

                pop     rax
                call    string

                jmp     .return

.percent:
                cmp     byte [rdx], '%'
                jne     .char

                mov     ah, '%'
                call    ascii

                jmp     .return

.char:
                cmp     byte [rdx], 'c'
                jne     .bin

                pop     rax
                shl     ax, 8
                call    ascii

                jmp     .return

.bin:
                cmp     byte [rdx], 'b'
                jne     .oct

                pop     rax
                call    bin

                jmp     .return

.oct:
                cmp     byte [rdx], 'o'
                jne     .hex

                pop     rax
                call    oct

                jmp     .return

.hex:
                cmp     byte [rdx], 'h'
                jne     .dec

                pop     rax
                call    hex

                jmp     .return

.dec:
                cmp     byte [rdx], 'd'
                jne     .end

                pop     rax
                call    dec

.return:
                inc     rdx
                jmp     .next

.end:
                push    rcx
                ret

_start:
                mov     rdx, output

                push    apology
                push    'D'
                push    50
                push    50
                push    50
                push    50
                push    argument

                call    printf

                mov     rax, 0x3C
                xor     rdi, rdi
                syscall


section .data

output          db 'I am %s who has skipped %d (%hh, %oo, %bb) days after %cEDline so now %s', 13, 10, '$'
argument        db "a very lazy guy", '$'
apology         db 'I am apologizing for misrespecting cats', '$'
