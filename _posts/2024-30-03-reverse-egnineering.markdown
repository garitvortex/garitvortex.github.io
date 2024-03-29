---
title: "file-run1 | Writeups"
date: 2024-03-25 20:14 +0300
categories: [Writeups]
tags: [Reverse Engineering, picoCTF2022]
---

# file-run1 | Instruction and Information

![Information and Instruction](../assets/images/file_run1.png)

AUTHOR: WILL HONG

Description
A program has been provided to you, what happens if you try to run it on the command line?
Download the program [here](https://artifacts.picoctf.net/c/218/run).

# file-run1 | Guide

According to the above instruction, I can try to run the program. Before running the program, I wanted to check if there is any packer used to this file, so I checked the file using Detect It Easy (Die), and the result is that the file contains pure C/C++ language.

![Packer checking on file .run](../assets/images/die_run1.png)

And usually (not in every CTFs), the flag can be strings inside the file, so, what I do is run

>strings "filename" --><br>
>strings run

![Strings in file .run](../assets/images/strings_run1.png)

and get the following result

>/lib64/ld-linux-x86-64.so.2<br>
>libc.so.6<br>
>printf<br>
>__cxa_finalize<br>
>__libc_start_main<br>
>GLIBC_2.2.5<br>
>_ITM_deregisterTMCloneTable<br>
>__gmon_start__<br>
>_ITM_registerTMCloneTable<br>
>u+UH<br>
>[]A\A]A^A_<br>
>picoCTF{****6_Y0Ur_F1r57_F***9*********} | We can see picoCTF{***} flag format<br>
>The flag is: %s<br>
>:*3$"<br>
>GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0<br>
>crtstuff.c<br>
>deregister_tm_clones<br>
>__do_global_dtors_aux<br>
>completed.8061<br>
>__do_global_dtors_aux_fini_array_entry<br>
>frame_dummy<br>
>__frame_dummy_init_array_entry<br>
>run.c<br>
>flag<br>
>__FRAME_END__<br>
>__init_array_end<br>
>_DYNAMIC<br>
>__init_array_start<br>
>__GNU_EH_FRAME_HDR<br>
>_GLOBAL_OFFSET_TABLE_<br>
>__libc_csu_fini<br>
>_ITM_deregisterTMCloneTable<br>
>_edata<br>
>printf@@GLIBC_2.2.5<br>
>__libc_start_main@@GLIBC_2.2.5<br>
>__data_start<br>
>__gmon_start__<br>
>__dso_handle<br>
>_IO_stdin_used<br>
>__libc_csu_init<br>
>__bss_start<br>
>main<br>
>__TMC_END__<br>
>_ITM_registerTMCloneTable<br>
>__cxa_finalize@@GLIBC_2.2.5<br>
>.symtab<br>
>.strtab<br>
>.shstrtab<br>
>.interp<br>
>.note.gnu.property<br>
>.note.gnu.build-id<br>
>.note.ABI-tag<br>
>.gnu.hash<br>
>.dynsym<br>
>.dynstr<br>
>.gnu.version<br>
>.gnu.version_r<br>
>.rela.dyn<br>
>.rela.plt<br>
>.init<br>
>.plt.got<br>
>.plt.sec<br>
>.text<br>
>.fini<br>
>.rodata<br>
>.eh_frame_hdr<br>
>.eh_frame<br>
>.init_array<br>
>.fini_array<br>
>.dynamic<br>
>.data<br>
>.bss<br>
>.comment<br>

From blockquotes above (result of taking strings from the ***.run*** file), we can find this string

> picoCTF{****6Y0Ur_F1r57_F***9*********}

which looks like a flag format from picoCTF, let's try to submit the flag, and *voila*, you just solve the challange
