### Quick Response? Execute!

![](media/image1.png){width="6.268055555555556in"
height="5.110416666666667in"}



Scanning the qr code gives the number 9876543210 -- not much help.

QR codes can be parsed using various tools such as cyberchef

Recipe: <https://gchq.github.io/CyberChef/#recipe=Parse_QR_Code(false)>

That gives the output;

\'=a\$#\"!\~}\|{zyxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMcbafed\]ba\`\_\^W{zZSXQ

uOTSLp3210/.-,+\*)(\'&%\$#\"!\~}\|{zyxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZvuWmrqponglkd\*

KJIHGFEDCBA@?\>=\<;:9876543210/.-,+\*)(\'&%\$#\"!\~}\|{zyxwvutsr0/.\',+\*)(!&%\|BAba\`\_\^

\]\\\[ZYXWVUTSRQPONMLKJI_dc\\a\`\_\^\]VUTx;:9876RQ3ONGFKJIBf)(\'&%\$#\"!\~}\|{zyxwvutsrqp

onmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLbgfedcba\`\_X\]Vz=SwWVOTMq43ON0LKJIHG@d\'&\<A

@?\>=\<;:3W7w/43,10)Mnmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJIHGFEDCBA@?\>=\<;:9876

LQPONMLKDIHAe?\>C\<;\_\"!\~}\|{3216/.3210)Mnmlkjihgfedcba\`\_\^\]\\rqputsrkpoh.ONMLKJ\`\_

dcba\`\_\^\]Vz=\<;:9876RQ3ONMFKJCBf)(\'CBA:?\>=\<5Yzyxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[Z

YXWVUTSRQPONMLKJIHGFEDCBA@?\>=\<;:9876543IHGLKJIHGFE\>=a\$#\"!\~}\|{zyxwvutsrqponml

kjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJIHdcba\`\_\^\]\\U=YRv9OTMRQJnNMFEiCHGFE\>b%\$#\"!\~

}\|{zyxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLhgf_dcba\`Y\^\]VzZYXWPOsSRQ3I

HGFEiIHAF?c&%\$:?\>=\<5:92Vwvutsrqponmlkjihgfe{z!\~}\|{zyxq7uWslqpoh.fkdihgf\_%FEa

ZYX\]\\\[TYRv9876543210/.-,+\*)(\'&%\$#\"!\~}\|{zyxwvutsrqponmlkjihgfedcba\`v{zyxwvutm

rqj0nPf,MLKJIHGFEDCBA@?\>=\<;:9876RQPONMLKJIBAe?\>CB;\_?!=\<;4Xyxwvutsrqponmlkjih

gfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJIHGFED\`BX\]V\[ZYXWVUNrRQPONMLKDh+\*)(\'&%\$#\"!\~}\|{z

yxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJIHGFEDCBA\]V\[ZYXWVUTMLp3210/.

-,+\*)(\'&%\$#\"!\~}\|{zyxwvutsrqponmlkjihgfedcba\`\_\^\]\\rqvutsrqponmf,MLhgfedcba\`Y}@

?\[ZYXW9UTSLpJnHMFKDhH\*@E\>=a\$:\^\>=\<5:92Vwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSR

QPONMLKJIHGFEDCBA@?\>=\<;:9876543210FKDCHGFEDCB;:\^!\~}\|{zyxwvutsrqponmlkjihgfed

cba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJ\`edcba\`\_X\]\\\[TxXW9UTMLp3210/.-,+\*)(\'&%\$#\"!76;:98765

4-Qrqponmlkjihgfedcba\`\_\^\]\\\[ZYutsrqponPfed\*KJIHGF\\\[\`\_\^\]\\\[ZYRv9876543210/.-,+\*

)(\>=BA@?\>=\<;:3Wxwvutsrqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPONMLKJIHGFEDCBA\]\\\[ZY

XW9ONSRKo210/.-,+\*)(\'&%\$#\"!=}5:9276543,Pqponmlkjihgfedcba\`\_\^\]\\\[ZYXWVUTSRQPON

MLKJIHGFEDCBA@?\>=\<RQPUTMRQPONMFjW

That looks random but there is a structure to it.

It's one of the esoteric programming languages that appear in CTFs and
(after being incorrectly convinced that it was a variant of befunge for
far too long) it turned out to be Malbolge

\~NB: I couldn't get it to run using <https://tio.run/> so lost a lot of
time here but

<http://www.malbolge.doleczek.pl/> did work

![](media/image2.png){width="6.268055555555556in"
height="4.0256944444444445in"}

The output is

<https://github.com/ThePhilosopherV/CTFs/raw/main/secret/BreakMe>

visiting this link this gives you a file called Breakme

running 'file' on it tells you that it's an ELF executable binary

![](media/image3.png){width="6.268055555555556in"
height="0.7409722222222223in"}

Tried running strings, debugging it, reverse engineering it (for hours!)
but eventually realised that it's a steganography challenge so looked
into steg and hiding things inside ELF binaries

A lot of reading/research/googling led me to steg86

<https://github.com/woodruffw/steg86>

![](media/image4.png){width="6.268055555555556in"
height="1.3055555555555556in"}

Installed that then ran it against the BreakMe file using

./steg86 extract BreakMe

![](media/image5.png){width="3.477425634295713in"
height="0.49584208223972004in"}

Which gave the flag

CTT{N1C1_W04K_M2T8_C0NG2T7}

[Solved.]{.underline}
