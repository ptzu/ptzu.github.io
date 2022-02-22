---
title: 快速傅利葉轉換 FFT
date: 2018-02-09 21:03:48
draft: false
tags: ["Template"]
categories: [解題區, Template, Math]
---


## Code
```cpp
const int N = 500005;
const double PI = acos(-1.0);

struct Virt{
    double r,i;
    Virt(double r=0.0,double i=0.0){
        this->r=r;
        this->i=i;
    }
    Virt operator+(const Virt &x){
        return Virt(r+x.r,i+x.i);
    }
    Virt operator-(const Virt &x){
        return Virt(r-x.r,i-x.i);
    }
    Virt operator*(const Virt &x){
        return Virt(r*x.r-i*x.i,i*x.r+r*x.i);
    }
};
//雷德算法--倒位序
void Rader(Virt F[],int len){
    int j=len>>1;
    for(int i=1;i<len-1;i++){
        if(i<j)swap(F[i],F[j]);
        int k = len >> 1;
        while(j>=k){
            j-=k;
            k>>=1;
        }
        if(j<k)j+=k;
    }
}
//FFT实现
void FFT(Virt F[],int len,int on){
    Rader(F, len);
    for(int h=2;h<=len;h<<=1) //分治后计算长度为h的DFT
    {
        Virt wn(cos(-on*2*PI/h),sin(-on*2*PI/h));  //单位复根e^(2*PI/m)用欧拉公式展开
        for(int j=0; j<len; j+=h)
        {
            Virt w(1,0);            //旋转因子
            for(int k=j;k<j+h/2;k++)
            {
                Virt u=F[k];
                Virt t=w*F[k+h/2];
                F[k]=u+t;     //蝴蝶合并操作
                F[k+h/2]=u-t;
                w=w*wn;      //更新旋转因子
            }
        }
    }
    if(on==-1)
    for(int i=0;i<len;i++)
    F[i].r/=len;
}
//求卷积
void Conv(Virt a[],Virt b[],int len){
    FFT(a,len,1);
    FFT(b,len,1);
    for(int i=0;i<len;i++)
        a[i]=a[i]*b[i];
    FFT(a,len,-1);
}
Virt va[N],vb[N];
int sum[N];
string operator*(string &a,string &b){
    memset(va,0,sizeof(va));
    memset(vb,0,sizeof(vb));
    memset(sum,0,sizeof(sum));
    int len1=a.length();
    int len2=b.length();
    int len=1;
    while(len<len1*2||len<len2*2)len<<=1;
    for(int i=0;i<len1;i++){
        va[i].r=a[len1-i-1]-'0';
        va[i].i=0;
    }
    for(int i=len1;i<len;i++){
        va[i].r=va[i].i=0;
    }
    for(int i=0;i<len2;i++){
        vb[i].r=b[len2-i-1]-'0';
        vb[i].i=0;
    }
    for(int i=len2;i<len;i++)
    vb[i].r=vb[i].i=0;
    Conv(va,vb,len);
    for(int i=len-1;i>=0;i--)
    sum[i]=(int)(va[i].r+0.5);
    for(int i=0;i<len;i++){
        sum[i+1]+=sum[i]/10;
        sum[i]%=10;
    }
    string str;
    int k=0;
    for(int i=len-1;i>=0;i--){
        if(sum[i]!=0){
            k=i;
            break;
        }
    }
    for(int i=k;i>=-0;i--)
    str+=(char)(sum[i]+'0');
    return str;
}
```
