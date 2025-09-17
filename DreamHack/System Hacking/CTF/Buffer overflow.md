
 38   printf ("the main function doesn't call win function (0x%lx)!\n", win);

win의 주소를 16진수로 찍으려는 의도

 47     printf ("|  %lx\t|  %16lx\t|\n", name + idx *8, *(long*)(name + idx*8));

첫번째 열: name 시작 주소에서 8바이트씩 건너뜀
두번쨰열: 해당 주소에 있는 8바이트를 읽어 16진수로 출력
