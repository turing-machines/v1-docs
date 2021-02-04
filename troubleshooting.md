# Troubleshooting

## I have networking issues 

If you have issues with the network on your please check the following:

1. Make sure the jumper switched to a boot mode
2. Check the ethernet switch and network chips for extra solder paste. If you see some tiny silver balls around the chip, just scrape them away with a needle or any other tool. 

![Extra solder material left on a network chip LAN9514](.gitbook/assets/image%20%2810%29.png)

![LAN9514 network chips locations on the board](.gitbook/assets/image%20%283%29.png)

![Network switch chip location on the board](.gitbook/assets/image%20%287%29.png)

## The nodes are boot looping

If the nodes on the Turing Pi 1 board are boot looping, there are few things you should check

1. Check if you use a proper power supply. You should use 12V, 5A, 60W power supply. Please read more about the power supply [here](get-started/power-supply.md).
2. The second thing that could cause this issue is if you run apps that require a lot of RAM and the total RAM on the board is insufficient. 



