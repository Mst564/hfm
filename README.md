# hfm
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define maxvalue 1000
typedef struct {
	int weight;
	int parent, lchild, rchild;
}HTNode, *HuffmanTree;
typedef char * * HuffmanCode;

void Select(HuffmanTree HT, int k, int &s1, int &s2){
	int m1, m2, i, j;
	for(i = 1; i < k; i++){
		m1 = m2 =maxvalue;
		s1 = s2 = 0;
		for(j = 1; j < k; j++){
		if(HT[j].weight<m1 && HT[j].parent == 0){
			m2 = m1;
			s2 = s1;
			m1 = HT[j].weight;
			s1 = j;
		}
		else if(HT[j].weight < m2 && HT[j].parent == 0){
			m2=HT[j].weight;
            s2=j;
		}	 
	}	
	}
}
void HuffmanCoding(HuffmanTree &HT,HuffmanCode &HC, int *w, int n){
	int m,i;
	char *cd;
	HuffmanTree p;
	if(n <= 1) return;
	m = 2*n-1;
	HT = (HuffmanTree)malloc((m+1)*sizeof(HTNode));
	p = (HuffmanTree)malloc((m+1)*sizeof(HTNode));
	for(p = HT, i=1; i <= n; ++i, ++p, ++w){
		 p[i].weight = *w;
		 p[i].parent = 0;
		 p[i].lchild = 0;
		 p[i].rchild = 0;}
	for( ; i <= m; ++i,++p){
		 p[i].weight = 0;
		 p[i].parent = 0;
		 p[i].lchild = 0;
		 p[i].rchild = 0;
	} 

	for(i = n+1; i <= m; ++i){//建赫夫曼树
		int s1, s2;
		Select(HT, i-1, s1, s2);
		HT[s1].parent = i;
		HT[s2].parent = i;
		HT[i].lchild = s1;
		HT[i].rchild = s2;
		HT[i].weight = HT[s1].weight + HT[s2].weight;
	}
	HC = (HuffmanCode)malloc((n+1)*sizeof(char *));//求编码
	cd = (char *)malloc(n*sizeof(char));
	cd[n-1] = '\0';
	for(i = 1; i <= n; ++i){
		int c,f,start;
		start = n-1;
		for(c = i, f = HT[i].parent; f!=0; c = f, f = HT[f].parent ){
			if(HT[f].lchild == c) cd[--start] = '0';
			else cd[--start] = '1';	
		}
		HC[i] = (char *)malloc((n-start)*sizeof(char));
		strcpy(HC[i],&cd[start]);
	}
	free(cd);
}
void main()
{
	HuffmanTree T;
	HuffmanCode C;
	int n = 4;
	int m[] = {7, 5, 2, 4};
	int * w = m;
	HuffmanCoding(*&T, C,  w,  n);
}
