---
title: "Safe Opener | picoCTF |CTF Writeups"
date: 2024-03-25 20:14 +0300
categories: [CTF Writeups]
tags: [Reverse Engineering, picoCTF2022]
---

# Safe Opener | Instruction and Information

![Information and Instruction](../assets/images/RE/safe_opener/safe_opener_ins.png)

AUTHOR: MUBARAK MIKAIL

Description
Can you open this safe?
I forgot the key to my safe but this [program](https://artifacts.picoctf.net/c/83/SafeOpener.java) is supposed to help me with retrieving the lost key. Can you help me unlock my safe?
Put the password you recover into the picoCTF flag format like:<br>
>picoCTF{password}

# Safe Opener | Guide

Straight out after downloading the program, we have file **SafeOpener.java** so, what I'm going to do is compile and run this file.<br>
Before compiling and running java file, make sure you have java development kit (***JDK***), you can install it using this command below:

>sudo apt update<br>
>sudo apt install default-jdk<br>
>sudo apt install default-jre<br>

after installing ***JDK***, I can compile and run the ***SafeOpener*** file,

>javac SafeOpener.java<br> (This command will create ***.class** java file that can be run)<br>

But, usually, I tend to read the code file first, before running it (with the intension and hope that maybe I will get hint or understand some or all of the code file flow). So, I concatenate the ***SafeOpener.java*** file (not the ***SafeOpener.class*** file), and get this following result:

![concatenating SafeOpener.java](../assets/images/RE/safe_opener/safe_opener_cat.png)

    import java.io.*;
    import java.util.*;
    public class SafeOpener {
        public static void main(String args[]) throws IOException {
            BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
            Base64.Encoder encoder = Base64.getEncoder();
            String encodedkey = "";
            String key = "";
            int i = 0;
            boolean isOpen;


            while (i < 3) {
                System.out.print("Enter password for the safe: ");
                key = keyboard.readLine();

                encodedkey = encoder.encodeToString(key.getBytes());
                System.out.println(encodedkey);

                isOpen = openSafe(encodedkey);
                if (!isOpen) {
                    System.out.println("You have  " + (2 - i) + " attempt(s) left");
                    i++;
                    continue;
                }
                break;
            }
        }

        public static boolean openSafe(String password) {
            String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";

            if (password.equals(encodedkey)) {
                System.out.println("Sesame open");
                return true;
            }
            else {
                System.out.println("Password is incorrect\n");
                return false;
            }
        }
    }

Simply, the program asked the user to input password with maximum of three attempts. And if the encoded base64 password is the same as the variable ***encodedkey***, the program will return true and prints "Sesame Open".<br>

We know from the Instruction above that the flag is the password that inputted inside flag format ***(picoCTF{password})***. And in order to retrieve the password, we just need to decode the strings from ***encodedkey*** variable (in this case I'm using online [base64 decode](https://www.base64decode.org/)), and we input that into the flag format to submit the flag. For validating if the password is correct, we can run the file and inputted the password.

![SafeOpener_decoded64](../assets/images/RE/safe_opener/safe_opener_decode.png)

![SafeOpener_flag](../assets/images/RE/safe_opener/safe_opener_flag.png)

>picoCTF{pl3as3_l#t_m#_1#t#_t#3_saf3}

And boom, *voila!*, you have another 100 points

