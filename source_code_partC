```asm
;==========================================================
; REAL-TIME CLOCK AND STOPWATCH USING 8051
;==========================================================

;----------------------------------------------------------
; 7-SEGMENT LOOKUP TABLE
;----------------------------------------------------------
                ORG     9000H

                DB      00111111B       ; 0
                DB      00000110B       ; 1
                DB      01011011B       ; 2
                DB      01001111B       ; 3
                DB      01100110B       ; 4
                DB      01101101B       ; 5
                DB      01111101B       ; 6
                DB      00000111B       ; 7
                DB      01111111B       ; 8
                DB      01101111B       ; 9


;----------------------------------------------------------
; DELAY SUBROUTINES
; Located at 8300H
;----------------------------------------------------------
                ORG     8300H

; SHORT_DELAY: Generates approximately 1 ms delay
SHORT_DELAY:
                MOV     R6, #23
SHORT_DELAY_1S_L2:
                MOV     R5, #45
SHORT_DELAY_1S_L3:
                DJNZ    R5, SHORT_DELAY_1S_L3
                DJNZ    R6, SHORT_DELAY_1S_L2
                RET


;----------------------------------------------------------
; DISPLAY DRIVERS
;----------------------------------------------------------

; SHOW_DISPLAY
; Input: A = digit value (0-9)
;        R5 = digit select mask
SHOW_DISPLAY:
                MOV     DPTR, #9000H
                MOVC    A, @A+DPTR

                MOV     DPTR, #0E800H   ; 8255 Port A
                MOVX    @DPTR, A

                MOV     A, R5
                MOV     DPTR, #0E801H   ; 8255 Port B
                MOVX    @DPTR, A
                RET


; MULTIPLEX_DISPLAYS: Refresh all six digits
MULTIPLEX_DISPLAYS:
                MOV     R7, #72

MULTIPLEX_LOOP:

; Digit 0: Hours Tens
                MOV     A, 15
                MOV     R5, #11011111B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

; Digit 1: Hours Units
                MOV     A, 14
                MOV     R5, #11101111B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

; Digit 2
                MOV     A, 13
                MOV     R5, #11110111B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

; Digit 3
                MOV     A, 12
                MOV     R5, #11111011B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

; Digit 4
                MOV     A, 11
                MOV     R5, #11111101B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

; Digit 5
                MOV     A, 10
                MOV     R5, #11111110B
                LCALL   SHOW_DISPLAY
                LCALL   SHORT_DELAY

                DJNZ    R7, MULTIPLEX_LOOP
                RET


;----------------------------------------------------------
; TIME MANAGEMENT
;----------------------------------------------------------

; RESET_STOPWATCH: Clear stopwatch time
; Addresses: 9003H-9005H
RESET_STOPWATCH:
                MOV     DPTR, #9003H
                MOV     A, #0
                MOVX    @DPTR, A

                INC     DPL
                MOVX    @DPTR, A

                INC     DPL
                MOVX    @DPTR, A
                RET


; RESET_CLOCK: Clear clock time
; Addresses: 9000H-9002H
RESET_CLOCK:
                MOV     DPTR, #9000H
                MOV     A, #0
                MOVX    @DPTR, A

                INC     DPL
                MOVX    @DPTR, A

                INC     DPL
                MOVX    @DPTR, A
                RET


; INCREMENT_STOPWATCH
INCREMENT_STOPWATCH:
                INC     21

                MOV     DPTR, #9005H
                MOVX    A, @DPTR
                INC     A

                MOV     B, #60
                DIV     AB
                MOV     08, A

                MOV     A, B
                MOVX    @DPTR, A

                MOV     A, 08
                JNZ     INCREMENT_STOPWATCH_INC_MIN
                RET


INCREMENT_STOPWATCH_INC_MIN:
                DEC     DPL
                MOVX    A, @DPTR
                INC     A

                MOV     B, #60
                DIV     AB
                MOV     08, A

                MOV     A, B
                MOVX    @DPTR, A

                MOV     A, 08
                JNZ     INCREMENT_STOPWATCH_INC_HR
                RET


INCREMENT_STOPWATCH_INC_HR:
                DEC     DPL
                MOVX    A, @DPTR
                INC     A

                MOV     B, #24
                DIV     AB

                MOV     A, B
                MOVX    @DPTR, A
                RET


; INCREMENT_CLOCK
INCREMENT_CLOCK:
                MOV     DPTR, #9002H
                MOVX    A, @DPTR
                INC     A

                MOV     B, #60
                DIV     AB
                MOV     08, A

                MOV     A, B
                MOVX    @DPTR, A

                MOV     A, 08
                JNZ     INCREMENT_CLOCK_INC_MIN
                RET


INCREMENT_CLOCK_INC_MIN:
                DEC     DPL
                MOVX    A, @DPTR
                INC     A

                MOV     B, #60
                DIV     AB
                MOV     08, A

                MOV     A, B
                MOVX    @DPTR, A

                MOV     A, 08
                JNZ     INCREMENT_CLOCK_INC_HR
                RET


INCREMENT_CLOCK_INC_HR:
                DEC     DPL
                MOVX    A, @DPTR
                INC     A

                MOV     B, #24
                DIV     AB

                MOV     A, B
                MOVX    @DPTR, A
                RET


;----------------------------------------------------------
; DISPLAY MODE HANDLER
; 9011H: Mode flag (0 = Stopwatch, 1 = Clock)
;----------------------------------------------------------
DISPLAY_MODE:
                MOV     R0, #6

                MOV     DPTR, #9011H
                MOVX    A, @DPTR

                MOV     R1, #10
                JNZ     DISPLAY_MODE_CLK


DISPLAY_MODE_SW:
                MOV     DPTR, #9005H
                SJMP    DISPLAY_STOPWATCH_LOOP


DISPLAY_MODE_CLK:
                MOV     DPTR, #9002H


DISPLAY_STOPWATCH_LOOP:
                MOVX    A, @DPTR
                MOV     B, #10
                DIV     AB

                MOV     @R1, B
                INC     R1

                MOV     @R1, A
                INC     R1

                DEC     DPL
                DJNZ    R0, DISPLAY_STOPWATCH_LOOP

                LCALL   MULTIPLEX_DISPLAYS
                RET


;----------------------------------------------------------
; TIMER 1 INTERRUPT VECTOR
;----------------------------------------------------------
                ORG     0FFF6H

                INC     20
                LJMP    TIMER_1S


;----------------------------------------------------------
; TIMER 1 SERVICE ROUTINE
;----------------------------------------------------------
                ORG     9100H

TIMER_1S:
                CLR     TR1

                MOV     TH1, #3CH
                MOV     TL1, #0B0H

                DJNZ    22H, TIMER_1S_CONTINUE

                MOV     22H, #25
                LJMP    TIMER_EVENT


TIMER_1S_CONTINUE:
                SETB    TR1
                RETI


; TIMER EVENT: Update time once per second
TIMER_EVENT:
                LCALL   INCREMENT_CLOCK

                MOV     16, DPH
                MOV     17, DPL

                MOV     DPTR, #9010H
                MOV     18, A
                MOVX    A, @DPTR

                JZ      TIMER_INT_NINC_SW

                LCALL   INCREMENT_STOPWATCH


TIMER_INT_NINC_SW:
                MOV     A, 18
                MOV     DPH, 16
                MOV     DPL, 17

                SETB    TR1
                RETI


;----------------------------------------------------------
; MAIN PROGRAM AND INITIALIZATION
;----------------------------------------------------------
                ORG     8100H

                MOV     A, #70H
                MOV     SP, A

; Configure 8255 PPI
                MOV     A, #10001001B
                MOV     DPTR, #0E803H
                MOVX    @DPTR, A

; Initialize Port C
                MOV     DPTR, #0E802H
                MOV     A, #0FFH
                MOVX    @DPTR, A

; Reset counters
                MOV     20, #0
                MOV     21, #0

                LCALL   RESET_STOPWATCH
                LCALL   RESET_CLOCK

; Configure Timer 1
                MOV     TMOD, #10H
                MOV     TH1, #3CH
                MOV     TL1, #0B0H
                MOV     22H, #25

                SETB    ET1
                SETB    EA
                SETB    TR1


;----------------------------------------------------------
; MAIN LOOP: BUTTON HANDLING AND DISPLAY UPDATES
;----------------------------------------------------------
ITERATE:

; Read mode switch (Port C, bit 1)
                MOV     DPTR, #0E802H
                MOVX    A, @DPTR
                MOV     20H, A

                JNB     20H.1, ITERATE_RESET_MODE

                MOV     DPTR, #9011H
                MOV     A, #1
                MOVX    @DPTR, A
                SJMP    ITERATE_MODE_END


ITERATE_RESET_MODE:
                MOV     DPTR, #9011H
                MOV     A, #0
                MOVX    @DPTR, A


ITERATE_MODE_END:

; Start/Stop button (Port C, bit 0)
                JNB     20H.0, ITERATE_RESET_SW

                MOV     DPTR, #9010H
                MOV     A, #1
                MOVX    @DPTR, A

                JB      21H.0, ITERATE_SW_END

                LCALL   RESET_STOPWATCH
                SETB    21H.0
                SJMP    ITERATE_SW_END


ITERATE_RESET_SW:
                MOV     DPTR, #9010H
                MOV     A, #0
                MOVX    @DPTR, A

                CLR     21H.0


ITERATE_SW_END:
                LCALL   DISPLAY_MODE
                SJMP    ITERATE

                END
```
