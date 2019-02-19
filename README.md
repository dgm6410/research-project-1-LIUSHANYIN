# research-project-1-LIUSHANYIN
research-project-1-LIUSHANYIN created by GitHub Classroom
https://shanyin.itch.io/fog-test
This is the link where you can domnload the test
this is the shader code

https://drive.google.com/drive/folders/12rpqc9ykx5MMbPKj_N72J2SxLkmXtQTj?usp=sharing
this is the second game link about the glass test
In order you make the glass real, I use shader to make the material.

Shader "Custom/Fog"
{
	Properties
	{
	
 
	_MainTex("Fog texture", 2D) = "white" {}
 _Mask("Mask", 2D) = "white" {}
	_Color("Color", color) = (1., 1., 1., 1.)
		
 
	_ScrollDirX("Scroll along X", Range(-1., 1.)) = 1.
		_ScrollDirY("Scroll along Y", Range(-1., 1.)) = 1.
		_Speed("Speed", float) = 1.
		_Distance("Fading distance", Range(1., 10.)) = 1.
	}
 
		SubShader
	{
		Tags{ "Queue" = "Transparent" "RenderType" = "Transparent" }
		Blend SrcAlpha OneMinusSrcAlpha
		ZWrite Off
		Cull Off
 
		Pass
	{
		CGPROGRAM
#pragma vertex vert
#pragma fragment frag
 
#include "UnityCG.cginc"
 
		struct v2f {
		float4 pos : SV_POSITION;
		fixed4 vertCol : COLOR0;
		float2 uv : TEXCOORD0;
		float2 uv2 : TEXCOORD1;
	};
 
	sampler2D _MainTex;
	float4 _MainTex_ST;
 
	v2f vert(appdata_full v)
	{
		v2f o;
		o.pos = UnityObjectToClipPos(v.vertex);
		o.uv = TRANSFORM_TEX(v.texcoord, _MainTex);
		o.uv2 = v.texcoord;
		o.vertCol = v.color;
		return o;
	}
 
	float _Distance;
	sampler2D _Mask;
	float _Speed;
	fixed _ScrollDirX;
	fixed _ScrollDirY;
	fixed4 _Color;
 
	fixed4 frag(v2f i) : SV_Target
	{
		float2 uv = i.uv + fixed2(_ScrollDirX, _ScrollDirY) * _Speed * _Time.x;
		fixed4 col = tex2D(_MainTex, uv) * _Color * i.vertCol;
		col.a *= tex2D(_Mask, i.uv2).r;
		col.a *= 1 - ((i.pos.z / i.pos.w) * _Distance);       
		return col;
	}
		ENDCG
	}
	}
