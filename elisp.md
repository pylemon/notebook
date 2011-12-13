;; chapter 1
;;==============================================================================

(+ 2 2 4)
; press C-x C-e at the end of the list will return 8 at minibuffer

'(this is quoted list)
; return a quoted list 

(this is a unquoted list)
; will return an error

(+ 2 (- 4 5 (* 4 6)))
; 2 + (4-5-(4*6)) = -23

fill-column
; return 70 it is an instance value

(+ 2 fill-column)
; return 72 use instance value to calculate

(concat "abc" "def")
; will concat the strings. use "" to quote string

(concat "The "  (number-to-string (+ 2 fill-column))  " red foxes.")
; use number-to-string to convert data types

(substring "The quick brown fox jumped." 16 19)
; return a substring of "fox" indexing from 16 to 19 starts from 0

(+) ; return 0 
(*) ; return 1

(message "My name is %s and I am %d years old." "pylemon" 25)

(message "He saw %d %s"	
	 (- fill-column 34)
	 (concat "red "	
		 (substring
		  "The quick brown foxes jumped." 16 21)
		  " leaping."))

(message "This buffer's name is: %s" (buffer-name))

(set 'fruite '(apple orange banana lemon))
(message "the fruite has: %s" fruite)
; result is the fruite has: (apple orange banana lemon)

(setq animal '(lion tiger leopard))
; use setq you don't need to write ' all the time

(setq trees '(pine fir oak maple)
      herbivores '(gazelle antelope zebra))
(message "%s%s" trees herbivores)
; you can write severval statment in one setq

(setq counter 0)
(setq counter (+ counter 1) )
counter
; once you eval (+ counter 1) counter incress 1


;; chapter 2
;;==============================================================================

; get file name or file path of buffer 

(buffer-name)
; "elisp.el"

(buffer-file-name)
; store the file path and filename of buffer
; "/home/liwei2/elisp.el"

;"/home/liwei2/elisp.el"
; use C-u C-x C-e make the return appear after the expression

(current-buffer)
;  =>   #<buffer elisp.el>
(other-buffer)
;  =>	#<buffer *Messages*>

(switch-to-buffer (other-buffer))
; switch-to-buffer is a function other-buffer is value kbd this func to some key
; binding that's how emacs works

(buffer-size)
; size of current buffer
(point)
; return the position of mouse. from the start of file to the mouse counting
; howmany chars in this area
(point-min) ; min possible value of point position 
(point-max) ; max possible value of point position


;; chapter 3
;;==============================================================================

(defun multiply-by-seven (number)
  "Multiply *number* by seven."
  (* 7 number))
(multiply-by-seven 2)
; call the function  return   =>  14

;; how-to write a function
; (defun func-name (argv)
;    "doc strings"
;    (interactive argv passing info) ; optional 
;    body...)


(if (> 5 4) 	; if part
    (message "5 is greater than 4")) 	; then part
; use if in elisp

(defun type-of-animal (characteristic)	; first version
  "if the CHARACTERISTIC is the symbol 'fierce',
   then warn of a tiger."
  (if (equal characteristic 'fierce)
      (message "It's a tiger")))

;; use if else
(defun type-of-animal (characteristic)	;second version
  "if the CHARACTERISTIC is the symbol 'fierce',
   then warn of a tiger."
  (if (equal characteristic 'fierce)
      (message "It's a tiger")
    (message "It's not fierce!")))

;; try to use func
(type-of-animal 'fierce)
(type-of-animal 'zebra)

(if 4
    'true
  'false)
;; 4 is true  => "true"

(if nil
    'true
  'false)
;; anything not nil is true  => "false"

(> 5 4)
;; => t  "t --> true"

(> 4 4)
;; => nil "nil --> false"

(defun back-to-indentation()
  "point to first visible character on line."
  (interactive)
(beginning-of-line 1)
(skip-chars-forward " \t"))

    fda fda
;; use this function to point to first chars

(let ((foo (buffer-name))
      (bar (buffer-size)))
(message "This buffer is %s and has %d characters!" foo bar))
; => "This buffer is elisp.el and has 3841 characters!"


(message "we are %d characters into this buffer."
    (- (point)
       (save-excursion
          (goto-char (point-min)) (point))))
; => "we are 4031 characters into this buffer."

(if (string= (int-to-string 23)
    (substring (emacs-version) 10 12))
(message "This is version 23 emacs")
(message "This is not version 23 emacs"))
; => "This is version 23 emacs"


; exercise of chapter 3
(defun double-number(number)
  "double the NUMBER given"
(interactive "p")
(* number 2))

(double-number 3)

(defun check-fill-column(number)
  "check whether the NUMBER give is greater than fill-column number"
(interactive "p")
(if (< number fill-column)
(message "The number is smaller than the fill-column %s < %s " number fill-column)
(if (> number fill-column)
(message "The number is greater than the fill-column %s > %s " number fill-column)
(if (= number fill-column)
(message "The number is equal to fill-column %s" number)))))

(check-fill-column 70)


;; chapter 4
;;==============================================================================

(defun simplified-beginning-of-buffer()
  "move the point to the beginning of buffer
leave mark at previous position"
(interactive)
(goto-char (point-min)))

(simplified-beginning-of-buffer)
; set a mark and goto beginning of buffer
(beginning-of-buffer)   
; it is the same func inside 


;; exercise of chapter 4

(defun simplified-end-of-buffer()
  "move the point to the end of buffer"
(interactive)
(goto-char (point-max)))

(simplified-end-of-buffer)

(defun simplified-get-buffer(buffername)
  "get buffer name whether if it is exists"
(interactive "BInput a buffer name: \n")
(if (get-buffer buffername)
(message "There is a buffer named %s " buffername)
(message "No buffer named %s" buffername)))
