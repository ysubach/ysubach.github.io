---
layout: post
title: "Eight queens (N-queens) solution in Clojure"
tags: [fp, clojure] 
---
I have been interested in learning functional programming language 
during last months. My current choice is [Clojure](http://clojure.org/)
language because this is a dialect of Lisp based on JVM. So it seems to
be good candidate for usage in real projects. 

Since functional programming requires thinking style
diffent from imperative languages like PHP, Python or Java I decided to
try and implement some pure computational task.

"Eight queens" task is perfect for testing functional programming
language because solution for this task is recursive. You need to put 8
queens on the chessboard so that no two queens attack each other. This
task can be extended to N-sized chessboard, in this case you need to
place N queens.

My solution in Clojure is written below:

<script src="https://gist.github.com/ysubach/8503969.js"></script>

{% comment %}
{% highlight clojure %}
(def board-size 8)
 
(defn qtest [qcol qvect]
  "Test if position `qcol` is okay for addition to `qvect`"
  (defn f [x] (let [[row col] x]
                (or
                  (= qcol col)
                  (= qcol (- col (+ row  1)))
                  (= qcol (+ col (+ row  1))))))
  (empty? (filter f (map-indexed vector (rseq qvect)))))
 
(defn qsolve [qvect]
  "Recursively find solution based on initial qvect` board"
  (doseq [p (range board-size)]
    (if (qtest p qvect)
      (let [qnew (conj qvect p)]
      (if (= (count qnew) board-size) (println qnew) (qsolve qnew))))))
 
(qsolve []) ; start solution finding from empty board config
{% endhighlight %}
{% endcomment %}

How to run this program: if you have Clojure and [Leiningen](http://leiningen.org/) installed
then save this code into `eight.clj` and run:

{% highlight shell %}
lein exec eight.clj
{% endhighlight %}

It will find all solutions and send them to console, one solution per
line (92 solutions for standard chessboard):

{% highlight text %}
[0 4 7 5 2 6 1 3]
[0 5 7 2 6 3 1 4]
[0 6 3 5 7 1 4 2]
[0 6 4 7 1 3 5 2]
[1 3 5 7 2 0 6 4]
[1 4 6 0 2 7 5 3]
[1 4 6 3 0 7 5 2]
...
{% endhighlight %}

Alternatively try to run the code through online execution service:
http://www.compileonline.com/execute_clojure_online.php

As result Clojure shows itself as a very expressive language. The same
conclusion is confirmed by 
[language expressivness study](http://www.infoq.com/news/2013/03/Language-Expressiveness).
And I hope to continue my journey to the functional programming world :-)
