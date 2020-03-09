# The Strategy Paatern (Policy Pattern)

**Classification: Behaviorial**

This provides a way to take a family of algorithms and encapusulate each one to make them interchanable with each other, Sinf the alg form a family, they will normally
operate with set inputs and outputs. 

The algorithms are allowd to vary independently and  the implentation can be quite different. 

Example: You are working on a system that processes customer orders. When it is time to ship the order, it's important to calculate te correct shipping cost
For exmaple use a shippinh cost calculator. 

The calcuator must support 3 shipping methods nd you should be able to add more easily. The 3 methods supported are:
- Fedex
- UPS
- Postal Service
