The Mask is the first byte in any **Blackwing** message. This byte tells the server what to expect. 
The LSB indicates whether the server should expect a traditional **Blackwing** message, containing a stamp and a letter, or another
type of message, for example, one starting with a session.

# Secure Mask

![alt text](figs/secure_mask.png)

## Asymetric algorithm choosing

![alt text](figs/secure_mask_rsa.svg)

## Symetric algorithm choosing

![alt text](figs/secure_mask_aes.svg)


# Unsecure Mask

![alt text](figs/unsecure_mask.svg)