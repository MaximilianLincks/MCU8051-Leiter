CSEG at 0h
;R3 register um die P0 Werte zu speichern
;R4 register um das Setzten/ Aufheben von Knöpfen zu überwachen
;R5 register für den Punktestand
;R6 register mit zähler für Refresh
;P0 ist die "Leiter"
;P1 sind die Tasten, wobei nur P1.0 (H) genutzt wird

LJMP init

init:

MOV P0, #11111101b

MOV P2,	#11111111b
MOV P3, #11111111b

MOV R3, P0
MOV R4, P1
MOV R5,	#00001010b
MOV R6, #00000001b

LJMP shiftl

shiftl:
;check if refresh needed
DEC R6
MOV A, R6
JZ callpunkte

MOV A, R3
RL A
MOV R3, A
MOV P0, R3;zeige die neue Position des Lichtes



MOV A, P1;momentaner Zusant des Knopfes
MOV B, R4;alter Zustand des Knopfes

CJNE A, B, resetleft
LJMP shiftr

shiftr:
;check if refresh needed
DEC R6
MOV A, R6
JZ callpunkteRight

MOV A, R3
RR A
MOV R3, A
MOV P0, R3;zeige neue Position des Lichts


MOV A, P1;momentaner Zusant des Knopfes
MOV B, R4;alter Zustand des Knopfes

CJNE A, B, resetright
LJMP shiftl

resetleft:
;momentan P1: #11111110b
MOV R4, P1
MOV A, R3
CJNE A, #01111111b, weiterl
LJMP pUP
weiterl:
LJMP shiftl

resetright:
MOV R4, P1
MOV A, R3
CJNE A, #11111110b, weiterr
LJMP pDown
weiterr:
LJMP shiftr


pUP:
MOV P0, #00000000b
INC R5
call punkte
MOV P0, #11111110b ;initialwert
MOV R3, P0
LJMP shiftl

pDOWN:
MOV P0, #11111111b
MOV A, R5
MOV B, #010b ;2
DIV AB
MOV R5, A
JNZ callpunkteDown
LJMP printlose

callPunkteRight:
call punkte
LJMP shiftr

callpunkteDown:
call punkte
MOV P0, #11111110b ;initialwert
MOV R3, P0
LJMP shiftl

callpunkte:
call punkte
LJMP shiftl

punkte:
MOV R6, #00000101b ;refresh counter
MOV DPTR, #table
;1er Stellen
mov p3, #11111110b
MOV A, R5
MOV B, #1010b  ;10
DIV AB
MOV A, B
movc A,@a+dptr
mov P2, A
;clear
mov p2, #11111111b
;10er Stellen
mov p3, #11111011b;100er Stellen
MOV A, R5
MOV B, #001100100b  ;100
DIV AB
movc A, @a+dptr
MOV P2, A;100er schreiben
mov p2, #11111111b
mov p3, #11111101b;10er Stellen
MOV A, B
MOV B,#1010b  ;10
DIV AB
movc A, @a+dptr
MOV P2, A
;clear
mov p2, #11111111b

RET

printlose:
MOV DPTR, #losetable
mov p3, #11110111b
MOV A, #0b
MOVC A, @a+dptr
MOV P2,A
;clear
mov p3, #11111111b
mov p2, #11111111b

mov p3, #11111011b
MOV A, #1b
MOVC A, @a+dptr
MOV P2,A
;clear
mov p3, #11111111b
mov p2, #11111111b

mov p3, #11111101b
MOV A, #10b
MOVC A, @a+dptr
MOV P2,A
;clear
mov p3, #11111111b
mov p2, #11111111b

mov p3, #11111110b
MOV A, #11b
MOVC A, @a+dptr
MOV P2,A
;clear
mov p3, #11111111b
mov p2, #11111111b

LJMP end

table:
db 11000000b
db 11111001b, 10100100b, 10110000b
db 10011001b, 10010010b, 10000010b
db 11111000b, 10000000b, 10010000b

losetable:
db 11000111b
db 11000000b, 10010010b, 10000110b 
end:
END

