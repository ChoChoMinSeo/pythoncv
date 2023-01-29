import cv2
import os
import numpy as np
from time import sleep
# curPath = os.path.abspath(os.path.dirname(__file__))
# IMAGEPATH = curPath+'/temp.jpg'

# IMAGEPATH = 'C:/TFOD/TFODC/PDF/test/mpl (7).jpg'
IMAGEPATH = 'C:/TFOD/TFODC/PDF/test/qw (1).jpg'
# IMAGEPATH = 'C:/TFOD/TFODC/PDF/test/asam (1).jpg'
start_x,start_y = -1,-1
end_x,end_y = -1,-1
isPressed = False

def draw(event, x,y, flag,param):
    global start_x,start_y,isPressed, end_x,end_y
    if event == cv2.EVENT_LBUTTONDOWN:
        isPressed = True
        start_x,start_y = x,y
    elif event == cv2.EVENT_MOUSEMOVE:
        if isPressed:
            temp = img.copy()
            cv2.rectangle(temp,(start_x,start_y),(x,y),(0,0,255),1)
            cv2.imshow('draw',temp)
    elif event == cv2.EVENT_LBUTTONUP:
        isPressed = False
        end_x,end_y = x,y
        cv2.rectangle(img,(start_x,start_y),(x,y),(0,0,255),1)
def getLine(croppedImg, startPoint):
    found = True
    baseY, floorY=-1,-1
    print(croppedImg.shape, startPoint)
    for i in range(startPoint, croppedImg.shape[0]):
        number_of_white_pix = np.sum(croppedImg[i][:] == 255)
        if(number_of_white_pix==croppedImg.shape[1]):
            if not found:
                floorY = i+10
                return croppedImg[baseY:floorY,:], floorY
            else:
                baseY = i-10
        else:
            found = False
def crop(event,x,y,flag,param):
    global curPoint, divider, tempLine
    end = curLine.shape[0]
    if event == cv2.EVENT_LBUTTONDOWN:
        cv2.line(tempLine,(x,0),(x,end),(0,0,255),1)
        cv2.imshow('crop',tempLine)
        divider.append(x)
    elif event==cv2.EVENT_RBUTTONDOWN:
        divider = [0]
        tempLine = curLine.copy()
        cv2.imshow('crop',tempLine)
original = cv2.imread(IMAGEPATH)
gray = cv2.cvtColor(original,cv2.COLOR_BGR2GRAY)
t, img = cv2.threshold(gray, -1, 255,  cv2.THRESH_BINARY | cv2.THRESH_OTSU)
img = cv2.resize(img,(0,0),fx=0.3,fy=0.3)
tempLine=[]
cv2.namedWindow('draw')
cv2.setMouseCallback('draw',draw)
cv2.imshow('draw',img)
while True:
    k=cv2.waitKey(1) & 0xFF
    if k ==27:
        break
    elif k == 13:
        cv2.destroyAllWindows()
        start_x =start_x*10//3
        start_y =start_y*10//3
        end_x =end_x*10//3
        end_y =end_y*10//3 
        curPoint=0
        croppedImg = original[start_y:end_y,start_x:end_x]
        croppedgray = cv2.cvtColor(croppedImg,cv2.COLOR_BGR2GRAY)
        t, croppedgray = cv2.threshold(croppedgray, -1, 255,  cv2.THRESH_BINARY | cv2.THRESH_OTSU)
        divider=[0]
        curLine, curPoint = getLine(croppedgray,curPoint)
        sleep(0.5)
        tempLine = curLine.copy()
        cv2.imshow('crop',tempLine) 
        cv2.setMouseCallback('crop',crop)
    elif k==32:
        try:
            divider.sort()
            for i in range(len(divider)-1):
                answerImg = curLine[0:curPoint,divider[i]:divider[i+1]]
                fileName = 'C:/TFOD/TFODC/PDF/answer/'
                answercount = len(os.listdir(fileName))+1
                fileName=fileName + str(answercount)
                fileName+='.jpg'
                cv2.imwrite(fileName,answerImg)
            curLine, curPoint = getLine(croppedgray,curPoint)
            tempLine = curLine.copy()
            divider.pop()
            for i in divider:
                cv2.line(tempLine,(i,0),(i,curLine.shape[0]),(0,0,255),1)
            cv2.imshow('crop',tempLine)        
        except:
            cv2.destroyAllWindows()
            cv2.imshow('draw',img)
            cv2.setMouseCallback('draw',draw)
cv2.destroyAllWindows()
