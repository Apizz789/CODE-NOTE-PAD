//Code by Abhishek Munagekar to implement  nary search
#include "iostream"
#include "omp.h"
int newar[100];
using namespace std;

int nary(int l,int r, int key,int n){
 int index = -1;
 int size=(r-l+1)/n;
 if(size==0 || n==1){
  #pragma omp parallel for
  for(int i=l;i<=r;i++){
   if(newar[i]==key) index=i;  
  }
 return index; 
 }
 int left=l;
 int right=r;
 omp_set_num_threads(n);
 omp_set_nested(1);
 # pragma omp parallel
 {
  int id=omp_get_thread_num();
  int lt=l+id*size;
  int rt=lt+size-1;
  if (id==n-1) rt=r;
  
  if(newar[lt]<=key && newar[rt]>=key){
   left=lt;
   right=rt;
  }  
 }
 if (left==l && right ==r) return -1;
 return nary(left,right,key,n);

}

int main(){
 
 
 
 cout<<"Program for nary search"<<endl; 
 int nos;
 int nthread;
 cout<<"Enter the number of numbers\t"<<endl;
 cin>>nos;
 cout<<"Please enter numbers one after the other\n";
 for(int i=0;i<nos;i++) cin>>newar[i];
 cout<<"Enter the number of theads\t";
 cin>>nthread;
 cout<<"Enter the key\t";
 int key;
 cin>>key;
 cout<<endl;
 cout<<nary(0,nos-1,key,nthread)<<endl;
 
 
  
}