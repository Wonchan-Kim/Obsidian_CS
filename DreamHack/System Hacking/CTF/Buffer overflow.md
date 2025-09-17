
 38   printf ("the main function doesn't call win function (0x%lx)!\n", win);

win의 주소를 16진수로 찍으려는 의도

 47     printf ("|  %lx\t|  %16lx\t|\n", name + idx *8, *(long*)(name + idx*8));

첫번째 열: name 시작 주소에서 8바이트씩 건너뜀
두번쨰열: 해당 주소에 있는 8바이트를 읽어 16진수로 출력

for (idx = 0; idx < 0x10; idx++) {
  printf("|  %lx\t|  %16lx\t|\n",
         name + idx * 8,                // 첫 번째 열: 주소
         *(long*)(name + idx * 8));     // 두 번째 열: 해당 주소의 8바이트 값을 long으로 해석해 16진수로
}

name은 16바이트 버퍼인데, 8바이트 단위로 16줄을 찍어서 스택 주변 메모리까지 보여줌

for (idx = 0; idx < count; idx++) {
  *(long*)(name + idx * 8) = value;
}

여기서 직접 입력받은 value 를 name 에 쓰기 때문에 위험함


win 함수에서 flag 읽어옴
win 함수의 주소: 0x000000000040125b
