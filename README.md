- 👋 Hi, I’m @allabakash486
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
allabakash486/allabakash486 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
# create a simulator

set ns [new Simulator]

#create trace file

set tracefile [open stable.tr w]

$ns trace-all $tracefile

#nam file creation

set namfile [open stable.nam w]

$ns namtrace-all $namfile

#finish procedure

proc finish {} {

global ns tracefile namfile

$ns flush-trace

close $tracefile

close $namfile

exec nam stable.nam &

exit 0

}

# node creation

set n1 [$ns node]

set n2 [$ns node]

# connection 

$ns duplex-link $n1 $n2 5Mb 2ms DropTail

#agent creation

set udp [new Agent/UDP]

$ns attach-agent $n1 $udp

set null [new Agent/Null]

$ns attach-agent $n2 $null

$ns connect $udp $null

#generate the traffic

set cbrstable [new Application/Traffic/CBR]

$cbrstable attach-agent $udp

#start traffic

$ns at 0.1 "$cbrstable start"

$ns at 4.5 "$cbrstable stop"

$ns at 5.0 "finish"

$ns run

