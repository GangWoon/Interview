
applifecycle을 공부하던 중 @autoreleasepool를 발견하고 의문점이 생겼다.

autorelease pool는 objc와 연관이 있어서 다른 블로그를 참고하면서 작성했다.

```
int main(int argc, char *argv[]) {
	@autoreleasepool {
    		return UIApplicationMain(...);
        }
}
```





> autorelease pool은 객체의 소유권을 포기할 수 있는 메커니즘을 제공하지만 즉시 해제 될 가능성을 방지한다. 



objc는 Swift와 달리 MRC(Manual Referencing Counting)을 사용한다.

retain(), release() 메소드를 사용해서 프로그래머가 직접 메모리 흐름을 제어한다.







아래 메소드를 보면 NSString alloc을 할 때 retain()이 불리며(rc +1)

외부에서 반환 값 label을 사용하면 rc가 2로 증가하게 된다.

문제점은 외부에서 반환 값을 release 시키면 rc가 1이 되는데

메소드 내부에서 증가시킨 rc를 줄일 방법이 없다는 것이다.

```
-(NSString *)getCoolLabel {
    NSString *label = [[NSString alloc] initWithString:@"SwiftRocks"];
    return label;
}
```







이런 방법을 해결하고자 autorelease가 등장하게 된다.

autorelease()는 미래 시점에서 해제되어야 할 풀에다가 추가한다.

기본적으로 해당 스레드의 RunLoop가 끝날 때 해제한다.

```
-(NSString *)getCoolLabel {
    NSString *label = [[NSString alloc] initWithString:@"SwiftRocks"];
    return [label autorelease];
}
```





해당 스레드의 RunLoop가 끝날 때 해제되긴 하지만 

아래와 같이 많은 양의 객체를 무작정 할당하는 건 메모리 낭비가 심해진다.

```
-(void)emojifyAllFiles {
    int numberOfFiles = 1000000;
    for(i=0;i<numberOfFiles;i++) {
        NSString *contents = [self getFileContents:files[i]];
        NSString *emojified = [contents emojified];
        [self writeContents:contents toFile:files[i]];
    }
}
```

![img](https://blog.kakaocdn.net/dn/bh8PON/btqIhZy9lP2/z53hlZVMj0Pxh2o6FmYdhk/img.png)





이 문제를 해결하기 위해서 @autoreleasepool 블록을 사용한다.

내부에서 정의된 autorelease 프로퍼티는 블록이 끝날 때 해제된다.

```
-(void)emojifyAllFiles {
    int numberOfFiles = 1000000;
    for(i=0;i<numberOfFiles;i++) {
        @autoreleasepool {
            NSString *contents = [self getFileContents:files[i]];
            NSString *emojified = [contents emojified];
            [self writeContents:contents toFile:files[i]];
        }
    }
}
```

![img](https://blog.kakaocdn.net/dn/bYIQ30/btqIpwwickC/NlwOlOa84pEI8hTBHdOeMK/img.png)



------



#### Reference

사진: [www.appleofeyes.com/엑스코드에서-크래쉬에-대처하기/](http://www.appleofeyes.com/엑스코드에서-크래쉬에-대처하기/)

내용: [medium.com/swift2go/autoreleasepool-uses-in-2019-swift-9e8fd7b1cd3f](https://medium.com/swift2go/autoreleasepool-uses-in-2019-swift-9e8fd7b1cd3f)