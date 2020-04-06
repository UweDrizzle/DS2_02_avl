#include <stdio.h> // 10727231 林曉凡 10727235 葉育瑋 
#include <stdlib.h>
#include <stdbool.h>
#include <cstdio>
#include <string.h>
#include <math.h> 
#include <fstream>
#include <iostream>
#include <vector>
#include <time.h>
using namespace std ;

struct Data{
	string name,departmentname,category,level,city,system;
	string student,teacher, code ,  departmentcode ;
	int graduate ;	
	int number ;
};

struct Node{
	vector<Data> school1;
	vector<Data> school2;
	Node *right;
	Node *left;
	Node *mid ;
	int height ;
};




class Job{
private:
	vector<Data> data;
	vector<Data> data2;
	Data o ;
	vector<Data> heap;
	
	string filenum ;
	int DataSize ;
public:
	
	
	
	
	Data D( string str ){
		int heapsize = 0 ;
		Data o ;
		char c = '\0' ;
		int i = 0 ;
		string str2 ;
		int t = 0 ;
		bool b = false ;
		str2 = "" ;
		bool tab = false ;
		o.number = DataSize + 1 ;
		while( str[i] != '\0' ) {
		
			if( str[i] == '\t' ) {
				t++ ;
			}
			if( str[i] != '\t' ) {
				//str2 = str2 + str[i] ;
				while( !tab && str[i] != '\0' ){
					if( str[i] != '\t' ){
						str2 = str2 + str[i] ;
						i++ ;
					}
					else {
						tab = true ;
						if( t == 1 )
							o.name = str2 ;
						else if( t == 3 )
							o.departmentname = str2 ;
						else if( t == 4 )
							o.category = str2 ;
						else if( t == 5 )
							o.level = str2 ;
						else if( t == 6 )
							o.student = str2 ;
						else if( t == 7 )
							o.teacher = str2 ;
						else if( t == 8 ) {
							o.graduate = atoi( str2.c_str()) ;
							b = true ;
							break ;
						}
						t++ ;
						str2 = "" ;
					}
				}
				
			}
			if( str[i] != '\0' ) 
				i++ ;
			tab = false ;
			if( b )
				break ;
			
		}

		return o ;
		
	}
	
	bool InputData( int h ){    // 將資料檔存放到動態陣列裡 
		string filename ;
		FILE *file ;
		int  n;
		DataSize = 0 ;
		
		cout << "請輸入檔案名稱\n" ; 
		cin >> filename ;
		filenum=filename;
		if ( h == 1 ) 
  			filename = "input" + filename + ".txt" ;
        //file.open( filename.c_str() ) ;
        file = fopen( filename.c_str(), "r" ) ;
        
        
  		if ( file == NULL ) { 
  			return false ;
  		} 
  		else { 
  			char CNDDCLCS[20] ;
  			char c = '\0' ;
  			
  			
  			for( int i = 1 ; i <=4 ; i++)  // 讀一開始的東西 
  				fscanf( file, "%s", CNDDCLCS) ;			
  			
  			for(int i=1;i<=11 ;i++){ // 讀一開始的東西 
  				fscanf( file, "%s", CNDDCLCS) ;			
			}			
			
			//cout << "\n";     
			string str1 ;
			string str2 ;
			string skip ;
			string str ;
			
			int graduate ;
			
			bool one = true ;
			//fscanf( file, "%c", &c ) ;
			while( fscanf( file, "%c", &c ) != EOF ) {
				str = "" ;
				if ( !one )
					str = str+c ;
				c = '\0' ;
				while( c != '\n' && fscanf( file, "%c", &c ) != EOF ) {
					if( c != '\n' )
						str = str + c ;
					
				}
				
				one = false ;
				o = D( str ) ;

    			data.push_back( o ) ;
    			DataSize++ ;
    			
				
			}
			   	
			fclose( file ) ;	
       	
       		return true;
		}		
		return false ;	
	}
	
    
	void deletevector(){ //清空VECTOR 
		data.clear() ;
		data2.clear() ;
	}
	
	vector<Data> R(){
		return data ;
	}

	
	
};


Node * LL( Node * x ) {  // LL rotate
	Node * y = x->left ;
	x->left = y->right ;
	y->right = x ;
	return y ; 
	
}


Node * RR( Node * x )  {  //  RR rotate
	Node * y = x->right ;
	x->right = y->left ;
	y->left = x ;
	return y ;
}

Node * LR( Node * x )  { // LR rotate
	Node * y = x->left ;
	Node * z = y->right ;
	y->right = z->left;
	x->left = z->right ;
	z->right = x ;
	z->left = y ;
	return z ;
} 

Node * RL ( Node * x ) { // RL rotate
	x->right = LL(x->right) ;
	return RR(x) ;
}

    Node * insert(int x, Node * t)  {
        if(t == NULL)
        {
            t = new Node;
            t->school1 = x;    //////////////////////////////////  ?_?
            t->height = 0;
            t->left = t->right = NULL;
        }
        else if(x < t->school1)   /////////////  ?_?
        {
            t->left = insert(x, t->left);
            if(height(t->left) - height(t->right) == 2)
            {
                if(x < t->left->data)
                    t = singleRightRotate(t);
                else
                    t = doubleRightRotate(t);
            }
        }
        else if(x > t->school1 )   /////////////////// ?_? 
        {
            t->right = insert(x, t->right);
            if(height(t->right) - height(t->left) == 2)
            {
                if(x > t->right->data)
                    t = singleLeftRotate(t);
                else
                    t = doubleLeftRotate(t);
            }
        }

        t->height = max(height(t->left), height(t->right))+1;
        return t;
    }






int RC(){ // 判別輸入值
	char str[]={0} ;
	scanf( "%s", str ) ;
	unsigned long long int i ;
	unsigned long long int num;
	for(i = 0; i <= strlen(str)-1 ; i++){
		if ( str[i]-'0'>9 || str[i]-'0'<0 ){
			return -99999 ;
		}
	}
	num = atoi(str);

	return num ;
}

int main(){
	
	
	int com = -1 ;
	printf( "請選擇要執行的指令\n" ) ; 
	printf( "0 結束\n1 min heap\n2 MinMaxHeap\n" ) ;
	com = RC() ;
	while (  com > 3 || com < 0 ) {
		printf( "指令錯誤  請重新輸入\n" ) ;
		com = RC() ;
	}
	while( com == 0 || com == 1 || com ==2 || com == 3) {
		if ( com == 1 || com == 2 || com == 3) {
			if ( com == 1 ) {
				//Mission1() ;  
			}
			else if ( com == 2 ) 
				//Mission2() ;  
		 
				 
			
			printf( "請選擇要執行的指令\n" ) ;
			printf( "0 結束\n1 min heap\n2 MinMaxHeap\n" ) ;
			com = RC() ;
			while (  com > 3 || com < 0 ) {
				printf( "指令錯誤  請重新輸入\n" ) ;
				com = RC() ;
			}
		}
		else if ( com == 0 ) {
			printf( "結束程式" ) ;
			com = 4 ;
		} 
	}

	return 0 ;	
}
