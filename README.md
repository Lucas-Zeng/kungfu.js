README
===========================
Kungfu.js（功夫）是一款处理setTimeout的工具。其主要目的在于提供方法链式调用setTimeout函数，并且能把一系列动作储存起来方便以后重调，避免程序员陷于深深的回调中.

****
###　　　　　　　　　　　　Author:Lucas-Zeng
###　　　　　　　　　 E-mail:247934556@qq.com

===========================

##<a name="index"/>目录
* [用法](#basic)
* [API介绍](#API)
    * [kungfu](#kungfu)
    * [hit](#hit)
    * [perform](#perform)
    * [find](#find)
    * [die](#die)
    * [rest](#rest)
* [实例](#eg)


##<a name="basic"/>用法

	kungfu( name ).hit( fn1 , timeout1 ).hit( fn2 , timeout2 )....perform();
	//name: string  动作的名称(可为空)
	//fn1, fn2, ... fnN: function  执行的动作
	//timeout1, tmieout2, ... timeoutN: number  延时触发的时间

######页面在1秒内闪烁一遍

	var shine = kungfu()
    	.hit( function(){
            $( 'body' ).css( 'opacity', '.5' );
        }, 250)
        .hit( function(){
            $( 'body' ).css( 'opacity', '0' );
        }, 250)
        .hit( function(){
            $( 'body' ).css( 'opacity', '.5' );
        }, 250)
        .hit( function(){
            $( 'body' ).css( 'opacity', '1' );
        }, 250);
    
    shine.perform();


##<a name="API"/>API 介绍


####<a name="kungfu"/>kungfu
	//初始化一个名字为LiXiaoLong的功夫对象。
    kungfu( 'LiXiaolong' );


####<a name="hit"/>hit
	//给功夫对象加上一个动作
    kungfu( 'LiXiaolong' ).hit( function(){
    	console.log( '寸拳!, 剩余90HP' );
    } , 1000 );
    
    //可以链式增加
    kungfu( 'LiXiaolong' ).hit( function(){
    	console.log( '寸拳!, 剩余90HP' );
    } , 1000 ).hit( function(){
    	console.log( '劈腿!, 剩余90HP' );
    }， 1000 );
    
	//hit的函数返回的值可以传递
    var fullHP = 100;
	kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    } , 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }， 1000 );



####<a name="perform"/>perform
	//执行定义好的功夫对象
    var fullHP = 100;
	kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    }, 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }, 1000 )
    .perform();
	//寸拳!, 剩余90HP
    //劈腿!, 剩余75HP
	
    //perform接受一个参数[function]，在动作完成后执行该参数
    var fullHP = 100;
	kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    }, 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }, 1000 )
    .perform(function(){
    	console.log( 'done' );
    });
    //寸拳!, 剩余90HP
    //劈腿!, 剩余75HP
    //done
    

####<a name="find"/>find
	//通过名称查找功夫对象
    kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    }, 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }, 1000 );
    
    kungfu.find( 'LiXiaolong' ).perform();
    //寸拳!, 剩余90HP
    //劈腿!, 剩余75HP



####<a name="die"/>die
	//中断正在执行的功夫对象
    var fullHP = 100;
	kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    }, 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }, 1000 )
    .perform();
    
    setTimeout( function(){
    	//在功夫执行到一般的时候执行die();
        kungfu.find( 'LiXiaolong' ).die();
    }, 1500 )
    
	//寸拳!, 剩余90HP


####<a name="rest"/>rest
	//停止动作，休息一段时间
    var fullHP = 100;
	kungfu( 'LiXiaolong' ).hit( function(){
    	var power = 10;
    	console.log( '寸拳!, 剩余' + ( fullHP - power ) + 'HP' );
    	return fullHP - power;
    }, 1000 ).rest( 1000 ).hit( function( remainingHP ){
    	//remainingHP = 90;
    	var power = 15;
    	console.log( '劈腿!, 剩余' + ( remainingHP - power ) + 'HP' );
        return remainingHP - power;
    }, 1000 )
    .perform();
    //寸拳!, 剩余90HP
    //劈腿!, 剩余75HP
    
    
##<a name="eg"/>实例：详见example/kungfu.html，以下为js摘要
	<script>
        kungfu( 'LiXiaolong' )
            .hit( LXL( '左勾拳' ) , 1000 )
            .hit( LXL( '右钩拳' ) , 1000 )
            .hit( LXL( '升龙拳' ) , 1000 )
            .hit( LXL( '无影脚' ) , 1000 )
            .hit( LXL( '踢' ) , 600 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '踢' ) , 200 )
            .hit( LXL( '渣渣!' ) , 1000 );

        kungfu( 'LuRenjia' )
            .hit( LRJ( '闪' ) , 1500 )
            .hit( LRJ( '痛' ) , 1000 )
            .hit( LRJ( '再闪' ), 1000 )
            .hit( LRJ( '!' ) , 1000 )
            .hit( LRJ( '-100HP' ) , 200 )
            .hit( LRJ( '-100HP' ) , 200 )
            .hit( LRJ( '-100HP' ) , 200 )
            .hit( LRJ( '-100HP' ) , 200 )
            .hit( LRJ( '-100HP(┬＿┬)' ) , 200 )
            .hit( LRJ( '-100HP(┬＿┬)' ) , 200 )
            .hit( LRJ( '-100HP(┬＿┬)' ) , 200 )
            .hit( LRJ( ' (┬＿┬) ' ) , 1300 );

        kungfu.find( 'LiXiaolong' ).perform();
        kungfu.find( 'LuRenjia' ).perform();
    </script>