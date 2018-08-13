# ViewPager+Fragment 实现滑动切换

建立框架，使用ViewPager+Fragment，可以达到滑动切换界面，并改变顶部标题栏与底部
导航栏的内容，并且可以通过底部导航栏四个部分的监听快速切换到对应界面

-----
![Picture1](/Modle/Pictures/Picture1.png)

## 所需布局文件

- 主界面布局(Main.xml)，顶部标题栏布局(Top.xml)，底部导航栏布局(bottom.xml)
- 在主界面使用include引用(Top.xml和Bottom.xml布局)
- 在主界面使用(ViewPager)布局，放在Top与Bottom中间
- 单独建立四个布局文件(tab01.xml,tab02.xml,tab03.xml,tab04.xml)

## 所需代码文件

### 1、主界面代码(MainActivity.java)

- 作用：导入顶部，底部，布局控件；
- 作用：添加四个tab布局到Viewpager，并设置适配；
- 以上两步就可以实现界面的滑动切换；
- 作用：设置ViewPager滑动监听；设置底部监听事件；
- 作用：通过底部或滑动监听切换界面和改变界面效果；
- **在主界面里不可以直接使用各个Fragment布局里面的控件**

### 2、每个(Fragment)界面代码

    public class tab02 extends Fragment {
        @Nullable
        @Override
        public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
            View main_tab02 = inflater.inflate(R.layout.tab02,container,false);
            return tab02;
        }

        /*
        * 在这里写这个界面的程序，实现这个界面的功能
        * ****/

    }



### 3、适配器代码(MyAdapter)

    public class MyAdapter extends FragmentPagerAdapter {
        private List<Fragment> fragList;
        private List<String> tabList;

        public main_Adapter(FragmentManager fm, List<Fragment> fragList){
            super(fm);
            this.fragList = fragList;
        }

        @Nullable
        @Override
        public CharSequence getPageTitle(int position) {
            return tabList.get(position);
        }

        @Override
        public Fragment getItem(int position) {
            return fragList.get(position);
        }

        @Override
        public int getCount() {
            return fragList.size();
        }

    }
