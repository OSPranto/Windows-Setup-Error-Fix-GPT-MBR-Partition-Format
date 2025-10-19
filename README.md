🔍 সমস্যা কোথায়?
Windows setup দিতে গেলেই অনেক সময় নিচের মত error আসে ⤵️  
> “Windows cannot be installed to this disk. The selected disk has an MBR partition style.”  
অথবা  
> “Windows cannot be installed to this disk. The selected disk has a GPT partition style.”

এই error-এর মূল কারণ — **USB bootable drive** আর **hard disk partition type** এক নয়।

👉 USB bootable (GPT / UEFI mode)
👉 Hard disk (MBR / Legacy mode)

এই mismatch এর কারণেই Windows setup দিতে গেলেই বলে:

এখন চল দেখি, hard disk format না দিয়েই কিভাবে এটা solve করা যায় 👇


🧩 Option 1: BIOS থেকে Legacy → UEFI তে switch করা (সবচেয়ে clean way)

যদি তোমার motherboard UEFI support করে, তাহলে একদম সহজ:

🔧 Step-by-step:
PC চালু করে BIOS/Setup এ ঢুকো (Del বা F2)
“Boot Mode”, “CSM”, “Legacy” এসব option খুঁজে বের করো
Boot Mode change করো → UEFI Only
Secure Boot থাকলে → Enable বা Auto রাখো
Save & Exit করো
এখন GPT USB দিয়ে boot দাও — এবার error দেবে না, সরাসরি Windows install হবে ✅

🧠 এই পদ্ধতিতে তোমার hard disk format দিতে হবে না, শুধু system টা GPT compatible করে ফেলছো।


🧩 Option 2: GPT USB → MBR compatible বানানো (যদি তোমার BIOS UEFI support না করে)

ধরো তোমার পুরনো PC, যেখানে UEFI নেই বা কাজ করছে না। তাহলে তুমি তোমার USB টাকেই MBR (Legacy bootable) করে নিতে পারো।

⚙️ Step-by-step:
Rufus খুলে USB select করো
Windows ISO ফাইল দাও
নিচে এইভাবে সেটিং দাও:
Partition Scheme → MBR
Target System → BIOS (or UEFI-CSM)
File System → NTFS
তারপর Start দাও।

🔹 এবার এই USB দিয়ে boot দিলে MBR hard disk এ Windows install হবে একদম আরামে — format লাগবে না।


🧩 Option 3: (Last resort) Hard disk কে GPT তে convert করা (without data loss)

যদি future-proof করতে চাও, MBR → GPT convert করতে পারো।
তবে risk থাকে, তাই backup নিয়ে করো।

Command Prompt (admin) থেকে:
mbr2gpt /convert /allowfullos


👉 এটা Windows 10/11 এ কাজ করে, আর data delete করে না।
কিন্তু BIOS mode UEFI না থাকলে এটা use কোরো না। 
