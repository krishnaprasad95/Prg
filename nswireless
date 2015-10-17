set val(chan)   Channel/WirelessChannel    ;# channel type

set val(prop)   Propagation/TwoRayGround   ;# radio-propagation 

model

set val(netif)  Phy/WirelessPhy            ;# network interface 

type

set val(mac)    Mac/802_11                 ;# MAC type

set val(ifq)    Queue/DropTail/PriQueue    ;# interface queue 

type

set val(ll)     LL                         ;# link layer type

set val(ant)    Antenna/OmniAntenna        ;# antenna model

set val(ifqlen) 50000                       ;# max packet in ifq

set val(nn)     20                        ;# number of 

mobilenodes

set val(rp)     AOMDV                       ;# routing protocol

set val(x)     1000                  ;# X dimension of 

topography

set val(y)      1000                     ;# Y dimension of 

topography

set val(stop)   10                    ;# time of simulation end

set val(energymodel) EnergyModel   ;

set val(initialenergy) 50 

set ROW 4

set COL 5

set MESSAGE_PORT 42

set BROADCAST_ADDR -1

#===================================

#        Initialization        

#===================================

#Create a ns simulator

set ns [new Simulator]

#Setup topography object

set topo       [new Topography]

$topo load_flatgrid $val(x) $val(y)

create-god $val(nn)

#Open the NS trace file

set tracefile [open aco.tr w]

$ns trace-all $tracefile

#Open the NAM trace file

set namfile [open ll.nam w]

$ns namtrace-all $namfile

$ns namtrace-all-wireless $namfile $val(x) $val(y)

set chan [new $val(chan)];#Create wireless channel

#===================================

#     Mobile node parameter setup

#===================================

$ns node-config -adhocRouting $val(rp) \

                -llType $val(ll) \

                -macType $val(mac) \

                -ifqType $val(ifq) \

                -ifqLen $val(ifqlen) \

                -antType $val(ant) \

                -propType $val(prop) \

                -phyType $val(netif) \

                -channel $chan \

                -topoInstance $topo \

                -agentTrace    ON \

                -routerTrace   ON \

                -macTrace      OFF \

                -movementTrace OFF \ 

-energyModel $val(energymodel) \ 

-initialEnergy $val(initialenergy) \

-rxPower 35.28e-3 \ 

-txPower 31.32e-3 \ 

-idlePower 712e-6 \ 

-sleepPower 144e-9 

#-RXThresh_ 2.78831e^-9

#===================================

#        Nodes Definition        

#===================================

for {set i 0} {$i < $val(nn) } {incr i} {

     set node($i) [$ns node]

$ns initial_node_pos $node($i) 35

}

#for {set i 0} {$i < $val(nn)} {incr i} {

# set myfirstagent($i) [new Agent/MyAgent]

# $node($i) attach $myfirstagent($i)

#}

#$myfirstagent(0) path

set mylistx {200.0 400 400.0 595 625 642.0 722.0 382.0 756.0 

228.0 105.4 225.9 248.2 425.3 172.2 618.3 465 396.2  296.3 196 

250 523.3 122.6 316 686 719.0 948 918.0 186.6 730.6 90.3 116.3 

269.1 276.3 433.3 217 602.33 688 728.1 468}

set mylisty {250 523.3 122.6 316 686 719.0 948 918.0 186.6 730.6 

90.3 116.3 269.1 276.3 433.3 217 602.33 688 728.1 468 200.0 400 

400.0 595 625 642.0 722.0 382.0 756.0 228.0 105.4 225.9 248.2 

425.3 172.2 618.3 465 396.2  296.3 196}

set mylistz {200.0 155.0 143.0 160.0 266.0 145.0 500.0 287.0 

177.0 199.0 230.0 212.1 199.2 205.3 177.9 166 231 120 234 221 

200.0 155.0 143.0 160.0 266.0 145.0 500.0 287.0 177.0 199.0 

230.0 212.1 199.2 205.3 177.9 166 231 120 234 221}

for { set j 0 } { $j < $val(nn) } { incr j } { 

$node($j) set X_ [lindex $mylistx $j] 

$node($j) set Y_ [lindex $mylisty $j]

        $node($j) set speed_ [lindex $mylistz $j] 

set mx [$node($j) set X_] 

set my [$node($j) set Y_] 

set mz [$node($j) set speed_] 

$ns at 0.0 "$node($j) setdest $mx $my $mz"

}

set tcp1 [new Agent/TCP/Newreno]

$tcp1 set class_ 1

set sink1 [new Agent/TCPSink]

$ns attach-agent $node(3) $tcp1

$ns attach-agent $node(5) $sink1

$ns connect $tcp1 $sink1

set ftp1 [new Application/FTP]

$ftp1 attach-agent $tcp1

$ns at 0.10 "$ftp1 start"

$ns at 9.0 "$ftp1 stop"

#Define a 'finish' procedure

proc finish {} {

    global ns tracefile namfile

    $ns flush-trace

    close $tracefile

    close $namfile

    exec nam ll.nam &

    exit 0

}

for {set i 0} {$i < $val(nn) } { incr i } {

    $ns at $val(stop) "\$node($i) reset"

}

$ns at $val(stop) "$ns nam-end-wireless $val(stop)"

$ns at $val(stop) "finish"

$ns at $val(stop) "puts \"done\" ; $ns halt"

$ns run

#1 14

#6 14

#3 11
