---
title: C언어 성적처리 프로그램 1.6
layout: post
tags: [C언어]
---
> **Update Notice**
>
>2024.02.12
>- 개인 기술 블로그 주소로 옮김
>
>2024.01.30
>- 신규 티스토리 블로그 주소로 옮김
>- 성적처리 프로그램 1.5 버전에 있는 내용을 추가하여 정리
>
>2016.07.24
>- 네이버 블로그에서 티스토리 블로그로 옮김
>
>2015.04.18
>- 명령행 인수를 두 개 입력을 해 놓아야 프로그램이 오류 없이 작동이 됨
>- 등수도 나오게 수정
>- 화면에 출력하는 것을 보기 좋게 조금 수정 
{: .prompt-info }

# C언어를 이용한 성적처리 프로그램 1.6
C언어를 공부하면서 문법 하나 끝낼 때마다 성적처리 프로그램 예제를 통해서 다졌는데 C언어 복습할 겸 다시 프로그램을 만들어 보았습니다.

이 프로그램은 파일 입출력, 구조체, 포인터, 함수, 제어문 등 중요한 문법들이 포함되어 있습니다. C 언어를 다 끝낼 수 없지만, 이 소스를 분석하면서 C 언어를 정리할 수 있지 않을까 생각합니다.

**성적처리 프로그램의 특징**
1. 성적 처리할 학생들의 정보들이 있는 텍스트 파일을 불러와 분석한다.
2. 100명 이하의 학생들을 처리할 수 있게 되어 있다.
3. 학생들의 학번, 이름, 국어, 영어, 수학 성적을 입력받는다.
4. 각 학생의 총점과 평균, 각 과목의 평균을 출력한다.
5. 출력은 총점이 높은 순(등수 순)으로 한다.

추가로 성적 처리 프로그램 결과를 파일에 저장하도록 수정했습니다. 입출력 파일은 명령행 인자로 받도록 하고 구조적인 프로그래밍으로 소스를 모듈화했습니다.

명령행 인자는 실행 시 원하는 파일을 선택해서 열거나 닫기가 가능하게 하는 것이고 구조적인 프로그래밍은 하나의 큰 프로그램을 여러 함수로 작은 구조로 모듈화하는 것입니다.

예를 들면, 스타워즈에 나오는 우주선을 레고로 만든다고 가정합시다. 무턱대고 작은 레고 블럭들을 가지고 처음부터 끝까지 만들면 시간도 걸리고 헷갈리고 빠진 부분도 있을 겁니다. 그렇다면 그 우주선을 구성하는 부분별로 먼저 만들고 다 합친다면 시간도 덜 걸리고 그 우주선의 구성도 처음 보는 사람들보다 눈에 익히게 될 겁니다. 이와 마찬가지로 큰 프로그램을 그 프로그램을 구성하는 기능별로 함수로 구현해서 구조적으로 만든다면 그 프로그램을 이해하기 편하고 만약에 어떤 기능에서 오류가 나타나면 그 부분만 수정하면 되니깐 프로그램 관리하기가 편해집니다.

이전 버전에서 명령행 인자로 파일 입력 받고, 결과를 파일에 저장하고, 기능별로 함수로 구현하여 다시 만들어 보았습니다. 너무 짧게(?) 프로그램을 수정하는 것 같아서 조금 찝찝하지만.... 그래도 약했던 포인터 부분과 함수 선언, 정의, 호출하는 걸 다시 공부할 수 있게 되어서 나름 좋았습니다.

~~다음 버전에는 크게 수정해서 메뉴 나오게 하고 여러 기능을 추가할 계획인데 할 수 있을지 모르겠네요... ㅋㅋㅋ~~

---

<img src="/assets/img/2015/03/14/image1.jpg" alt="성적처리 프로그램 실행 화면"/>

```c
#define _CRT_SECURE_NO_WARNINGS

/*  성적처리 프로그램  */

/* 주의 사항 : 해당 프로그램을 실행하기 전에는
 반드시 해당 작업 디렉토리에 sungjuk.txt 파일을 포함하고 실행하세요.
 명령행 인수에 sungjuk.txt out.txt 추가해서 실행하세요.   */

/* sungjuk.txt 파일 내용
 * 11201 남이석 10 37 10
 * 11200 강유비 100 97 100
 * 11202 정영돈 12 20 20
 * 11203 김재동 90 98 90
 * 11204 유새윤 80 80 44
 * 11205 방명수 30 30 30
 * 11206 유제석 66 60 60
 * 11207 강오동 50 50 52
 * 11208 조언석 40 100 40
 * 11209 심동엽 70 32 70
*/

#include <stdio.h>    //printf(), scanf()
#include <stdlib.h>    //exit()

typedef struct SCORE {   //구조체 SCORE형을 score형으로 재정의(선언)
    char num[10];    //학번
    char name[10];    //이름
    int kor,eng,math,tot;    //국어, 영어, 수학, 총점
    double avg;    //평균
}score;

int Data_input(FILE *in, score *people, score *total);    //Data_input함수 선언
void sort_select(int i, score *people);            //선택 정렬함수 선언
void swap(score *pn, score *pm);    //swap함수 선언
void Print_out(int i, score *people, score *total);    //화면 출력하는 함수 선언
void File_Print_out(int i, score *people, score *total, FILE *out);    //파일에 결과를 저장하는 함수 선언

int main(int argc, char *argv[])    //명령행 인자 추가
{
    //변수 선언
    int i=0;
    FILE *in, *out;        //입력파일 포인터 선언
    score people[100];    //score형 배열 선언
    score total={"","",0,0,0,0,0.};    //score형 total 변수 초기화


    //파일 입출력
    in=fopen(argv[1],"rt");    //입력 파일을 명령행 인자로 받는다
    if(in==NULL){   //파일이 제대로 열렸는지 확인하는 작업
        puts("해당 파일을 찾을 수 없습니다.");
        exit(1);    //파일이 제대로 열리지 않으면 프로그램을 끝냄
    }

    out=fopen(argv[2],"wt");    //출력 파일을 평령행 인자로 쓴다
    if(out==NULL) puts("해당 파일을 찾을 수 없습니다."), exit(1);


    //성적 처리할 학생들의 데이터 분석
    i=Data_input(in,people,&total);

    //파일을 다 읽었으면 닫음
    fclose(in);

    //선택 정렬(selection sort)
    sort_select(i, people);

    //화면에 결과 출력
    Print_out(i,people,&total);
    
    //화면에 출력된걸 파일에 입력하여 저장한다
    File_Print_out(i,people,&total,out);

    //파일을 다 썼으면 닫음
    fclose(out);

    return 0;
}


void swap(score *pn, score *pm)    //score형 매개변수 2개를 교환하는 함수
{
    score temp;    //임시 저장 변수
    temp=*pn;
    *pn=*pm;
    *pm=temp;
}

int Data_input(FILE *in, score *people, score *total)
{
    //구조체 포인터 변수 선언, call by reference 구현
    //포인터 변수는 주소를 참조

    int i=0;

    while(1){
        if(feof(in)) break;    //EOF를 검출하면 0이 아닌 값을 리턴
        fscanf(in,"%s %s %d %d %d\n",people[i].num,people[i].name,&people[i].kor,&people[i].eng,&people[i].math);
        //위 두 식을 아래와 같이 한줄로 써도 무방하다
        //if(fscanf(in,"%s %s %d %d %d\n",people[i].num,people[i].name,
        //                                    &people[i].kor,&people[i].eng,&people[i].math)==EOF) break;

        people[i].tot=people[i].kor+people[i].eng+people[i].math;    //각 학생의 총점 계산
        people[i].avg=people[i].tot/3.;                                //각 학생의 평균 계산
        total->kor=total->kor+people[i].kor;        //국어 점수의 평균을 계산하기 위한 모든 학생의 점수 더함
        total->eng=total->eng+people[i].eng;        //영어 점수의 평균을 계산하기 위한 모든 학생의 점수 더함
        total->math=total->math+people[i].math;        //수학 점수의 평균을 계산하기 위한 모든 학생의 점수 더함
        total->tot=total->tot+people[i].tot;        //총점의 평균을 계산하기 위한 모든 학생의 총점 더함
        total->avg=total->avg+people[i].avg;        //평균의 평균을 계산하기 위한 모든 학생의 평균 더함

        i++;    //다음 학생
    }

    return i;
}

void sort_select(int i, score *people)
{
    int n,m;

    for(n=0;n<i-1;n++){
        for(m=n+1;m<i;m++){
            if(people[n].tot<people[m].tot){   //자신과 다음 학생의 총점을 비교하여 자신 점수가 더 낮을 경우
                swap(&people[n], &people[m]);    //swap함수를 호출하여 자리를 바꾼다
            }
        }
    }
}


void Print_out(int i, score *people, score *total)
{
    int j;

    printf("\n                성적처리(등수순)            \n\n");
    printf("----------------------------------------------------------------------\n");
    printf("   등수    학번        이름      국어    영어    수학    총점    평균\n");
    printf("----------------------------------------------------------------------\n");
    //학생들의 정보 출력
    for(j=0;j<i;j++){
        printf("%5d %9s %12s %7d %7d %7d %7d  %7.2lf\n",j+1,people[j].num,people[j].name,
            people[j].kor,people[j].eng,people[j].math,people[j].tot,people[j].avg);
    }
    printf("----------------------------------------------------------------------\n");
    //학생들의 모든 점수, 총점, 평균 합한 결과 출력
    printf("%15s %12s %7d %7d %7d %7d  %7.2lf\n",total->num,total->name,
            total->kor,total->eng,total->math,total->tot,total->avg);
    //모든 점수, 총점, 평균의 평균 출력
    printf("%15s %12s %7.2lf %7.2lf %7.2lf %7.2lf  %7.2lf\n",total->num,total->name,
            total->kor/(double)i,total->eng/(double)i,total->math/(double)i,
            total->tot/(double)i,total->avg/(double)i);
    printf("----------------------------------------------------------------------\n");
}

void File_Print_out(int i, score *people, score *total, FILE *out)
{
    int j;

    fprintf(out,"\n                성적처리(등수순)            \n\n");
    fprintf(out,"----------------------------------------------------------------------\n");
    fprintf(out,"   등수    학번        이름      국어    영어    수학    총점    평균\n");
    fprintf(out,"----------------------------------------------------------------------\n");
    for(j=0;j<i;j++){
        fprintf(out,"%5d %9s %12s %7d %7d %7d %7d  %7.2lf\n",j+1,people[j].num,people[j].name,
            people[j].kor,people[j].eng,people[j].math,people[j].tot,people[j].avg);
    }
    fprintf(out,"----------------------------------------------------------------------\n");
    fprintf(out,"%15s %12s %7d %7d %7d %7d  %7.2lf\n",total->num,total->name,
            total->kor,total->eng,total->math,total->tot,total->avg);
    fprintf(out,"%15s %12s %7.2lf %7.2lf %7.2lf %7.2lf  %7.2lf\n",total->num,total->name,
            total->kor/(double)i,total->eng/(double)i,total->math/(double)i,
            total->tot/(double)i,total->avg/(double)i);
    fprintf(out,"----------------------------------------------------------------------\n");
}
```
<br>
**참고**

도서: 제대로 배우는 C언어 프로그래밍, 한성현 저자, 글로벌