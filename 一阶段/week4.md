
�ֱ��ʣ�ָ����Ϻ͸߶����������ʾ���������ص����
��ࣺ����������֮��ľ��롣�������Ļ��С�����˷ֱ��ʴ�С
DPIÿӢ�������ܶ�
PPI��Ļ�����ܶ�

XHTM��HTML����
	XHTMԪ�ر�����ȷǶ�ף�Ԫ�ر���պϣ���ǩ����Сд�������и�Ԫ��

text-transform��capitaize ÿ����������ĸ��д
font-variant:small-caps�����ı�����ΪС�ʹ�д��ĸ

��λ rem���ɸ��ݸ�Ԫ��html��font-size��С�����㣬�������СΪ15px����1rem=5px

ճ�Զ�λ��position��sticky���������topֵ��λ
	

Doctype���ã������ĵ����ͣ��������������֪��Ӧ�����ĸ��淶�������ĵ���������html��һ�У�����html��ǩ
	�ϸ�ģʽ�����������w3c��׼�������롣�ĵ�����doctype��Ϊ�ϸ�ģʽ
	����ģʽ������ģʽ�����ģʽ��ָ����������Լ��ķ�����������

��ҳ�Ż���
	1��H1��ǩ��ʹ�ã���������Ȩ�ر�������ǩ�ߣ������ڰ���logo
	2��ҳ��ͷ���Ż�
	   <meta name="keywords" content=""/>��վ�ؼ��֣���Ҫ���ù��࣬��������һ��ֻ�������ǰ�����ؼ���
	    <meta name="description" content=""/>������վ,
	3���������Ż���Ϊ���洴�����õ�����ͨ��
	     Ϊa����title���ԣ��ȿ�������ʾ�ÿ͵����ã�Ҳ��������������֪����Ҫȥ����
	     ��ò�Ҫʹ��ͼƬ�ȵ�����
	4��ͼƬ�Ż�
	     ʹ��alt������title����
	

ý���ѯ
 div{
	height:200px;
	backgound-color:red;
}
/*������Ļ�����800-1000֮�����ʽ*/     all // only screen
@media all and (min-width:800px) and (max-width:1000px){
      div{
	height:200px;
	backgound-color:red;
      }
}

/*����*/
@media only screen (orientation:portrait){
      ָ����ʽ
}
/*����*/
@media only screen (orientation:portrait){
      ָ����ʽ
}


����  transition:all 2s 2s;
	��һ��ֵ��transition-property:��Ҫ���ɵ����ԡ��������֮�䶺�Ÿ���
	�ڶ���ֵ��transition-duration����ʱ��
	������ֵ��transition-delay�ӳ�ʱ��
	transition-timing-function�������� linear(),ease(),ease
	���ɶ�display:none;��������

λ�� transform��50px 100px;
	λ�� transform: translate(x,y)��ˮƽ/��ֱ������λ��
		translateX()
		translateY()
		translateZ()��z�����λ��
	        transform: translate3d(x,y,z) 3dλ��

	��ת  transform: rotate��45deg,45deg��
		rotateX(45deg)ˮƽ����
		rotateY(45deg)��ֱˮƽ����
		rotateZ(45deg)z�᷽��
	         transform:rotate3d(value,value,value,deg);valueȡֵΪ0��1,1��ʾ��ת��0��ʾ����ת�����ĸ�ֵ����ת�Ƕ�ֵ

	����  transform: scale(1,2);ֵ����һ�Ŵ�С��һ��С
		scaleX(1)ˮƽ����
		scaleY(2)��ֱ����
		scaleZ();
		scale(1.5)ֻ��һ��ֵ��ʱ���ʾ��������һ������

	transform-origin(value1,value2)�����˶����յ�,Ҳ����ȡֵtop,right,left,nottom��Ĭ�ϵ���Ԫ������

	�ռ�����  transform-style:flat(Ĭ��,2d)/preserve-3d(3d);

	perspective:500px;͸��(����)���ԣ�����۲���תЧ�������д�ڸ�Ԫ�����档��Ԫ��д��transform:perspective(500px);
	
	����transformͬʱ����ʱ����һ���Ḳ��ǰһ��

����animation ������Ҫ��
	1��������animation-name
	2������ִ���� @keyframes myname{    //myname�Ƕ���Ķ�������
				from{ }  //����д�ٷ���0%~100%
				to{ }
				}
	3����������ʱ��animation-duration:3s;

        ������������animation-timing-function:
	linear����
	ease
	ease-in��������
	ease-out�ɿ쵽��
	ease-in-out�����쵽��
	**step-start������������ÿһ����֡��״̬

          �����ӳ�ʱ��animation-delay��2s;

          ����ѭ������animation-iteration-count:infinite(���޴�)/number;

          ����ѭ���Ƿ���animation-direction
	nomal:��������
	reverse:������
	alternate:�����󷴣��������������
	alternate-reverse:�ȷ������������������

           ���ö��󶯻�״̬animation-play-state:running(�˶�)/paused(��ͣ)

     �������� animation:mtmove 2s ease-in-out 2s infinite alternate;
	��������ʱ�䣬�������ͣ��ӳ�ʱ�䣬ѭ����������������


animation������transition���ɵ�����
	��ͬ�㣺��������ʱ��ı�Ԫ�ص�����ֵ
	��ͬ�㣺transition��Ҫ�����¼�(hover//click��)������ʱ��ı�css������ֵ
	 	animation���败��Ҳ������ʱ����ʾ�ĸı�css����ֵ


���Խ���  ��һ��������һ������
	 background:linear-gradient(to left,red,yellow)  ���ҵ����ɺ���
	�ǶԽ� background:linear-gradient(to left bottom,red,yellow)�����½ǵ����Ͻǡ�ָ������ֵ�������ң�������
	�Ƕ� background:linear-gradient(45deg,red,yellow) 0degʱ�Ǵ������� 

���򽥱� �����ĵ����ܽ��䣬���м�������
	background:radial-gradient(ellipse,red,blue,yellow)


Ԫ�ش�ֱ���У��������������λ����϶�λʵ��Ԫ�ؾ���

һ��Ԫ�������ҳ������ʾ����Ԫ�����ͺͺ�ģ�;���
	��ģ�ͣ�css���ֵĶ���ͻ�����λ
	Ԫ�����ͣ���display���ԣ���ͬ����ֵ������ͬ�����ͣ��Ӷ�����Ԫ�ز�ͬ��Ⱦ��ʽ

BFC�鼶��ʽ�����ģ�����Ԫ����ζ������ݽ��ж�λ���Լ�������Ԫ�صĹ�ϵ���໥����
	BFC������1������Ԫ�أ�float����none�����ֵ
		2��position��ֵΪabsolute��fixed
		3��display��ֵΪinline-block��table-cell��table-caption
		4��overflow����visible�����ֵ
	BFCӦ�ã�1�����������������
		2������Ӧ�������֣�����bfc��ֹԪ�ر�����Ԫ�ظ��ǵ�������ʵ������Ӧ�������֣�
		      ��������һ�������󸡶����ڶ�������overflow:hidden;
		3�����marginֵ�ص�����




