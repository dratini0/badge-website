# Using the Grove Serial connector

The Badge Team thoughtfully chose to include a couple of
Seeed Studio Grove system connectors to the 2018 badge. Looking at the
rear of the badge, they are either side of the unpopulated 3.5mm jack
footprint, top right.

- On the left is the Grove I2C connector \<anyone using this?\>
- On the right is the Grove UART connector

The Grove UART is helpfully labelled "UART4". To use it in Python, it is
actually number 2!!!

Example:

```python
import machine
u = machine.UART(2, 9600)
g = u.readline()

```
etc...

can be used to communicate with the Seeed Studio GPS module. [Seeed
Studio](http://wiki.seeedstudio.com/Grove-GPS/)
