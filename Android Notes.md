## 1. Chronometer Class##
+ Chronometer.start()
+ Chronometer.stop()
+ Chronometer.setFormat("Test %s")  
Format string: if specified, the Chronometer will display this string, with the first "%s" replaced by the current timer value in "MM:SS" or "H:MM:SS" form. If no format string is specified, the Chronometer will simply display "MM:SS" or "H:MM:SS".
+ Chronometer.setBase(SystemClock.elapsedRealtime())
