# HideObjectsFromUnderground
Here's a shader for Unity that hides objects from underground (-Y)

# Setup 
Download HideObjectsFromUnderground shader
This shader uses a discard statement to discard fragments that have a world normal pointing downwards (-Y direction), effectively hiding objects from underground. You can adjust the condition to discard fragments based on other criteria as well.

The shader also includes a texture sample to avoid any compilation errors, but you can remove it if you don't need it. Just make sure to update the properties accordingly.



## OR Simply Copy this code to your Custom Lit Shader

Shader "Custom/HideFromUnderground" {
    Properties{
        _MainTex("Texture", 2D) = "white" {}
        _Color("Color", Color) = (1,1,1,1)
        _Cutoff("Alpha cutoff", Range(0,1)) = 0.5
    }

        SubShader{
            Tags {"Queue" = "Transparent" "RenderType" = "Transparent"}

            Pass {
                Cull Off
                CGPROGRAM
                #pragma vertex vert
                #pragma fragment frag
                #include "UnityCG.cginc"

                struct appdata {
                    float4 vertex : POSITION;
                    float2 uv : TEXCOORD0;
                };

                struct v2f {
                    float2 uv : TEXCOORD0;
                    float4 vertex : SV_POSITION;
                    float worldPos : TEXCOORD1;
                };

                sampler2D _MainTex;
                float4 _MainTex_ST;
                float4 _Color;
                float _Cutoff;

                v2f vert(appdata v) {
                    v2f o;
                    o.vertex = UnityObjectToClipPos(v.vertex);
                    o.uv = TRANSFORM_TEX(v.uv, _MainTex);
                    o.worldPos = mul(unity_ObjectToWorld, v.vertex).y;
                    return o;
                }

                // Hides objects from below (Y < 0)
                fixed4 frag(v2f i) : SV_Target {
                    if (i.worldPos < 0) {
                        discard;
                    }

                    fixed4 col = tex2D(_MainTex, i.uv) * _Color;
                    clip(col.a - _Cutoff);
                    return col;
                }
                ENDCG
            }
        }
            FallBack "Diffuse"
}

