# diameter-of-a-tree-using-dp

#include<bits/stdc++.h>
using namespace std;
vector<int>tree[20001];
int diameter[20001];
int downPath[20001];



void eva_downPaths(int src, int par){
    int leaf = 1;
    int temp = 0;
    for(auto child : tree[src]){
        if(child!=par){
            leaf=0;
        eva_downPaths(child, src);
        temp = max(temp, downPath[child]);
            
        }
    }
    
    if(leaf==1){
        downPath[src]=0;
    }
    
    else{
        downPath[src] = 1 + temp;
    }
    
}


 


void solve(int src, int par){
    vector<int>childrenDownPath;
    
    int ans = 0;
    
    for(auto child : tree[src]){
        if(child!=par){
            solve(child, src);
            childrenDownPath.push_back(downPath[child]);
           ans = max(ans, diameter[child]);
        }
    }
    
        int num = childrenDownPath.size();

    
    sort(childrenDownPath.begin(), childrenDownPath.end());
    
    if(num==0){
        diameter[src]=0;
    }

    else if(num==1){
        diameter[src] = 1  + childrenDownPath[0];
    }
    
    else{
        diameter[src] = 2 + childrenDownPath[num-1] + childrenDownPath[num-2];
    }
    
    diameter[src] = max(diameter[src], ans);
}

int main(){
    int n;
    cin >> n;
        for(int i=1;i<n;i++)
        {
            int u,v;
            cin >> u >> v;
            tree[u].push_back(v);
            tree[v].push_back(u);
        }
        
        
        eva_downPaths(1,0);
        solve(1,0);
        cout << diameter[1];
        return 0;
}
