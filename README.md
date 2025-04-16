# Digital-clock
Racket 

#lang racket

(require web-server/servlet
         web-server/servlet-env)

(define (start req)
  (response/xexpr
   `(html
     (head
      (title "Digital Clock")
      (style
       "body { font-family: sans-serif; text-align: center; margin-top: 100px; }
        #clock { font-size: 48px; color: #2c3e50; }"))
     (body
      (h1 "Digital Clock")
      (div ((id "clock")) "00:00:00")
      (script
       "
        function updateClock() {
          const now = new Date();
          const hours = String(now.getHours()).padStart(2, '0');
          const minutes = String(now.getMinutes()).padStart(2, '0');
          const seconds = String(now.getSeconds()).padStart(2, '0');
          document.getElementById('clock').innerText = `${hours}:${minutes}:${seconds}`;
        }
        setInterval(updateClock, 1000);
        updateClock(); // call once immediately
       ")))))


(define-values (serve-clock _)
  (serve/servlet start
                 #:launch-browser? #t
                 #:servlet-path "/"
                 #:port 8080
                 #:servlet-regexp #rx""))

