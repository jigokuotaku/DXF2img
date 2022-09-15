import matplotlib.pyplot as plt
import ezdxf
from ezdxf.addons.drawing import RenderContext, Frontend
from ezdxf.addons.drawing.matplotlib import MatplotlibBackend
import glob
import re
import sys
import os
import matplotlib as mpl
import argparse
from argparse import RawTextHelpFormatter

class DXF2IMG(object):

  default_img_format = '.png'
  default_img_res = 1200
  def convert_dxf2img(self, names, img_format=default_img_format, img_res=default_img_res):
    for name in names:
      doc = ezdxf.readfile(name)
      msp = doc.modelspace()
      auditor = doc.audit()
      if len(auditor.errors) != 0:
        raise exception("The DXF document is damaged and can't be converted!")
      else :
        fig = plt.figure()
        ax = fig.add_axes([0, 0, 1, 1])
        ctx = RenderContext(doc)
        ctx.set_current_layout(msp)
        ezdxf.addons.drawing.properties.MODEL_SPACE_BG_COLOR ='#FFFFFF'
        out = MatplotlibBackend(ax)
        Frontend(ctx, out).draw_layout(msp, finalize=True)

        img_name = re.findall("(\S+)\.",name)  # select the image name that is the same as the dxf file name
        first_param = ''.join(img_name) + img_format  #concatenate list and string
        fig.savefig(first_param, dpi=img_res)
        print(name," Converted Successfully")

def using_argv(argv):
    """"""
    des = '''Example:
      dxf2img{0} -h
      dxf2img{0} -f ?.dxf
      dxf2img{0} -f ?.dxf -t pdf
      dxf2img{0} -f ?.dxf -t jpg -d "Microsoft JhengHei"
      dxf2img{0} -f ?.dxf -d "Microsoft JhengHei"
    '''
    if os.name=="nt":
      des=des.format(".exe")
    else:
      des=des.format("")
     
    parser = argparse.ArgumentParser(
      prog='Dxf to PDF',
      formatter_class=RawTextHelpFormatter,
      description=des
    )
    
    parser.add_argument('-f', '--file', dest='file', help="DXF File Path", type=str,default='*.dxf')
    parser.add_argument('-t', '--type', dest='type', help="Output File Type:[PDF,PNG,JPG,TIF,SVG]\nDefault:PDF", type=str,default='PDF')
    parser.add_argument('-d', '--default', dest='default', help='defaultFamily["ttf"]:["Microsoft JhengHei","DFKai-SB"]\nDefault:"DFKai-SB"', type=str,default='DFKai-SB')
    return parser

if __name__ == '__main__':

    args=using_argv(sys.argv[1:])
    options, rest = args.parse_known_args(sys.argv[1:])
    print("options:",options)
    print("rest:",rest)

    print(f"Command Line: {sys.argv}")
    print(f"DXF File Path: {options.file}")
    print(f"Output File Type: {options.type}")
    print(f"defaultFamily['ttf']: {options.default}")
    print(mpl.matplotlib_fname())
    print(mpl.get_configdir())
    print(mpl.get_backend())
    mpl.rcParams['font.family'] = options.default #'sans-serif'
    mpl.rcParams['font.sans-serif'] = options.default #'Microsoft JhengHei'
    mpl.font_manager.fontManager.defaultFamily["ttf"]=options.default
    print(mpl.font_manager.fontManager.defaultFamily["ttf"])
    first =  DXF2IMG()
    first.convert_dxf2img([options.file],img_format='.'+options.type)
