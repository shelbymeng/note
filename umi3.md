## 路由  
### 配置路由  
```
export default {
    routes: [
        { path: './路径名', component: '组件路径'}
    ]
}
```  
**注意：**  
component中可以使用绝对路径也可以时相对路径，若为相对路径，会从src/page找起。  
如果指向sec目录的文件，可以用`@`也可以用`../`，更推荐用前者。  

配置子路由，通常是需要为多个路径增加layout组件。  
```
export default {
    routes: [
        {
            path: './', 
            component: '@/layouts/index',
            routes: [
                {path: './list', component: 'list'},
                {path: './admin', component: 'admin'},
            ],
        },
    ],
}
```  
在`src/layouts/index`中通过`props.children`渲染子路由。  
```
export default (props) => {
    return <div>{ props.children }</div>
}
```  
此时访问/list和/admin就会带上src/layouts/index这个layout组件。  
### 页面跳转  
```
import { history } from 'umi';  
//跳转指定路由  
history.push('./list');  
//带参数跳转  
history.push('./list?a=b');
history.push({
    pathname: './list',
    query: {
        a: 'b'
    },
})
//跳转上一个路由  
history.goBack();  
//对应react组件接收  
props.location.query
```   
### Link组件  
```
import { Link } from 'umi'  
export default () => {
    <div>
        <Link to='/users'>User Page</Link>
    </div>
}
```  
**注意：**  
Link只用于单页面应用的内部跳转，若是外部跳转则使用a标签。  
### 路由组件参数  
路由组件可以通过props获取到属性。  
- match，当前路由和url match后的对象,`params,path,url,isExact`属性。  
- location,表示应用当前出于哪个位置，`pathname,search,query`。  
- history,同history。  
- route，当前路由的配置，`path,exact,component,routes`  。  
- routes,全部的路由信息。  
### 给子路由传递参数  
通过cloneElement，一次完成。  
```
export default function layout(props){
    return React.Children.map(props.children, child => {
        return React.cloneElement(child, {foo: 'bar'});
    });
}
```  