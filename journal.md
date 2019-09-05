__Day 1__:
- tracing algorithms
- logic gates
- blockhain
- ROT13
- basic \*NIX terminal knowledge
- ASL (American Sign Language)

__Day 2__:

We had the task to make a bash script to calculate the value of `E`. We had to use conditionals, loops, etc. Here is my solution to the problem:
```lang=bash
stat=()
his=(0 0 0 0 0 0)

read -p "Input x: " x

for (( i = 0; i < x; i++ )); do
    read -p "(0 ≤ int ≤ 5) stat[$i] = " stat[i]
done

values="${stat[@]}"

for value in $values; do
    ((sum=his[$value]+1))
    his[$value]=$sum
done

((E=his[1] + 2*his[2] + 3*his[3] + 4*his[4] + 5*his[5]))

echo "stat: ${stat[*]}"
echo "his: ${his[*]}"
echo "E: $E"
```

__Day 3__:

We went over basic network topologies by size and connections. After reading an article about this we did an exercise on the whiteboard to †est our knowledge.

For practising what we just learnt we connected to the Computer Science class raspberry pies via ssh like this:
`ssh pi@192.168.4.150`

We ssh'ed from that machine to another one with the same command. Also, we practised more bash scripting by creating a file with our name with CLI text editor `nano`.
