---
title: Применение кодировщиков и декодеров изображений в управляемом GDI+
ms.date: 03/30/2017
helpviewer_keywords:
- image encoders [Windows Forms], using
- image decoders [Windows Forms], using
ms.assetid: 0e838ea1-4e7e-4334-b882-ab25df607b8b
ms.openlocfilehash: 8cd66f3ce3da462867da9e23c38b3f6d877c058c
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67505089"
---
# <a name="using-image-encoders-and-decoders-in-managed-gdi"></a>Применение кодировщиков и декодеров изображений в управляемом GDI+
<xref:System.Drawing> Пространство имен предоставляет <xref:System.Drawing.Image> и <xref:System.Drawing.Bitmap> классы для хранения и управления изображениями. С помощью кодировщиков изображений в GDI +, можно сохранять изображения из памяти на диск. С помощью декодеров изображений в GDI +, можно загружать изображения с диска в память. Кодировщик преобразует данные в <xref:System.Drawing.Image> или <xref:System.Drawing.Bitmap> объект в формат файла выделенном диске. Декодер преобразует данные в файл на диске в формате, необходимом <xref:System.Drawing.Image> и <xref:System.Drawing.Bitmap> объектов.  
  
 GDI + имеет встроенных кодировщиков и декодеров, которые поддерживают следующие типы файлов:  
  
- BMP  
  
- GIF  
  
- JPEG  
  
- PNG  
  
- TIFF  
  
 GDI + также имеет встроенные декодеры, которые поддерживают следующие типы файлов:  
  
- WMF  
  
- EMF  
  
- ЗНАЧОК  
  
 Кодировщики и декодеры, более подробно рассматривается в следующих разделах:  
  
## <a name="in-this-section"></a>В этом разделе  
 [Практическое руководство. Получение списка установленных кодировщиков](how-to-list-installed-encoders.md)  
 Описывает способы просмотра кодировщикам, которые доступны на компьютере.  
  
 [Практическое руководство. Получение списка установленных декодеров](how-to-list-installed-decoders.md)  
 Описывает способы просмотра декодеров на компьютере.  
  
 [Практическое руководство. Определение параметров, поддерживаемых кодировщиком](how-to-determine-the-parameters-supported-by-an-encoder.md)  
 Описывает способы просмотра <xref:System.Drawing.Imaging.EncoderParameters> поддерживаемых кодировщиком.  
  
 [Практическое руководство. Преобразовать изображение из формата BMP в формат PNG](how-to-convert-a-bmp-image-to-a-png-image.md)  
 В этой статье описывается сохранение изображения в различный формат изображения.  
  
 [Практическое руководство. Установка уровня сжатия JPEG](how-to-set-jpeg-compression-level.md)  
 В этой статье описывается изменение уровень качества изображения.  
  
## <a name="reference"></a>Ссылка  
 <xref:System.Drawing.Image>  
  
 <xref:System.Drawing.Bitmap>  
  
 <xref:System.Drawing.Imaging.ImageCodecInfo>  
  
 <xref:System.Drawing.Imaging.EncoderParameter>  
  
 <xref:System.Drawing.Imaging.Encoder>  
  
## <a name="related-sections"></a>Связанные разделы  
 [Управляемый код GDI+](about-gdi-managed-code.md)  
  
 [Изображения, точечные рисунки и метафайлы](images-bitmaps-and-metafiles.md)
