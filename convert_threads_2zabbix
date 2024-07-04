#!/usr/bin/python3
import json
import sys


f = sys.stdin
data = json.load(f)
print("[")
lista = data["stats"]["threads"]
for i,interface in enumerate(lista):
    if "W#" in interface:
        if i:
            print(",")
        print("{\"{#THREAD}\":\""+interface+"\"}",end="")
print("\n]")
